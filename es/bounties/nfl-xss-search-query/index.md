---
layout: default
---

<div class="container">
<article>
  <a href="../../../es/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>NFL: XSS Reflejado en Parámetro de Búsqueda</h1>
    <p style="color: var(--p3); font-weight: 600;">National Football League (NFL) - Infraestructura Digital</p>
  </div>

  <div class="bounty-content">
    <h2>🎯 Resumen de la Vulnerabilidad</h2>
    <p>Se descubrió que la funcionalidad de búsqueda principal de la NFL era vulnerable a Cross-Site Scripting (XSS) reflejado. La vulnerabilidad residía en cómo la aplicación manejaba y reflejaba el parámetro de búsqueda en la página sin una codificación de salida adecuada.</p>

    <h2>⚙️ Detalles Técnicos</h2>
    <ul>
      <li><strong>URL Afectada:</strong> <code>https://www.nfl.com/search/?query=[XSS_PAYLOAD]</code></li>
      <li><strong>Parámetro Afectado:</strong> <code>query</code></li>
      <li><strong>Vector:</strong> Inyección de scripts maliciosos a través de la URL.</li>
    </ul>

    <h2>🛠️ Pasos para Reproducir</h2>
    <p>Al inyectar una etiqueta de script estándar en el parámetro <code>query</code>, confirmamos que la entrada se reflejaba directamente en el Document Object Model (DOM):</p>
    
    <pre><code>"><script>alert(document.domain)</script></code></pre>
    
    <img src="../../../assets/images/nfl-xss-search-query/PayloadExecution.png" alt="Ejecución de Payload XSS">

    <p>La aplicación no saneaba la entrada, permitiendo que el script se ejecutara en el contexto del navegador de la víctima. Esto podría aprovecharse para robar información de sesión sensible o realizar acciones en nombre del usuario.</p>

    <h2>⚠️ Impacto de Seguridad</h2>
    <p>Un atacante podría diseñar un enlace malicioso y enviarlo a un usuario autenticado. Una vez pulsado, el atacante podría:</p>
    <ul>
      <li>Exfiltrar cookies de sesión y eludir la autenticación.</li>
      <li>Realizar acciones no autorizadas (impacto similar a CSRF a través de XSS).</li>
      <li>Alterar el contenido del sitio web para usuarios específicos.</li>
    </ul>

    <h2>🤝 Detalle de Colaboración</h2>
    <p>Esta investigación fue realizada en una colaboración conjunta:</p>
    <ul>
      <li><strong>{{ site.researchers.ivan.name }}</strong> (Investigador Principal)</li>
      <li><strong>{{ site.researchers.diego.name }}</strong> (Analista de Seguridad)</li>
    </ul>

    <div class="card-grid" style="margin-top: 3rem;">
      <div class="glass-card">
        <small>Estado</small>
        <strong>Resuelto y Verificado</strong>
      </div>
      <div class="glass-card">
        <small>Severidad</small>
        <strong>P3 (Media)</strong>
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
