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
    <a href="../../../en/bounties/nfl-xss-search-query/" class="lang-btn">English (EN)</a>
    <a href="./" class="lang-btn active">Español (ES)</a>
  </div>

  <div class="bounty-header">
    <h1>NFL: XSS Reflejado en Búsqueda</h1>
    <p>Descubrimiento y explotación de parámetros de búsqueda sin escapar en los activos digitales de la National Football League.</p>
  </div>

  <div class="bounty-content">
    <h2>🔍 Investigación de Vulnerabilidad</h2>
    <p>Las pruebas de reflexión en <code>hbcutournament.nfl.com</code> revelaron que la entrada a través del parámetro <code>?q=</code> se renderizaba directamente en el DOM sin sanitización.</p>
    <p><code>https://hbcutournament.nfl.com/resources?q=</code></p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20HTML%20Injection.png" alt="Confirmación de Inyección HTML">

    <h2>🚨 Ejecución del Vector XSS</h2>
    <p>Se logró la ejecución de JS arbitrario mediante etiquetas <code>img</code> sin origen, activando el controlador de eventos <code>onerror</code>.</p>
    <pre><code>&lt;img/src/onerror=alert(8)&gt;</code></pre>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Alert%201.png" alt="XSS Activado">

    <h2>🚀 Exfiltración Crucial de Datos</h2>
    <p>Al encadenar el XSS con <code>fetch()</code>, logré exfiltrar con éxito los datos de <code>document.cookie</code> a un servidor controlado, demostrando el potencial destructivo de un secuestro de sesión.</p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration.png" alt="Secuestro de Cookies">
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration%20Result.png" alt="Logs del Servidor Recibidos">

    <h2>🤝 Detalle de Colaboración</h2>
    <p>Esta investigación fue realizada en una colaboración al 50% entre {{ site.researchers.ivan.name }} y {{ site.researchers.diego.name }}.</p>
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
