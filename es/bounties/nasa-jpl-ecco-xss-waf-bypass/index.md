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
    <h2>🔍 Descubrimiento Inicial</h2>
    <p>El objetivo fue el activo del Jet Propulsion Laboratory de la NASA <code>ecco.jpl.nasa.gov</code>. Las pruebas en la funcionalidad de búsqueda revelaron la reflexión de la entrada dentro de un contexto JavaScript.</p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn</code></p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal.png" alt="Reflexión de Búsqueda">

    <h2>🛡️ Estrategia de Evasión de WAF</h2>
    <p>El WAF bloqueaba inicialmente las etiquetas <code>&lt;script&gt;</code>. Al romper la lógica de <code>console.log</code> y codificar los puntos y coma como <code>%3b</code>, logré ejecutar JS arbitrario.</p>
    <pre><code>pwn'); // Rompemos la lógica del sistema
a=document; // Creamos la var 'a' que contiene document
alert(a.cookie); // Llamada al alert con cookies
&lt;/script&gt;;// // Cerramos tag y comentamos el resto</code></pre>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Bypass%20Semicollon.png" alt="Bypass de punto y coma">

    <h2>🚀 Payload de Exploit Final</h2>
    <p>La PoC final exfiltró cookies vía <code>fetch()</code> a un servidor externo. El bypass implicó el uso de referencias de variables para evitar cadenas directas de <code>document.cookie</code> que estaban filtradas.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration.png" alt="PoC de Exfiltración">

    <h2>🎖️ Reconocimiento y Premio</h2>
    <p>NASA JPL reconoció esta divulgación con una Carta de Reconocimiento oficial, confirmando el impacto y la remediación de la vulnerabilidad.</p>
    <div class="pdf-container">
      <iframe src="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf#toolbar=0" width="100%" height="100%" frameborder="0"></iframe>
    </div>
    <p style="text-align: center; margin-top: 20px;">
      <a href="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" class="lang-btn" target="_blank">📥 Descargar Carta Oficial (PDF)</a>
    </p>

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
