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
    <h1>XSS Reflejado en Buscador con Bypass de Filtros</h1>
    <p style="color: var(--p3); font-weight: 600;">
      National Aeronautics and Space Administration – Jet Propulsion Laboratory (NASA JPL)
    </p>
  </div>

  <div class="bounty-content">

    <h2>🔍 Identificación Inicial</h2>
    <p>
      Durante la evaluación de seguridad del dominio público correspondiente a JPL,
      se analizó el comportamiento del formulario de búsqueda.
    </p>

    <p>
      Se detectó que el parámetro de búsqueda reflejaba directamente la entrada del usuario
      dentro del código JavaScript renderizado en el navegador.
    </p>

    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal.png" alt="Búsqueda Normal">

    <p>
      El valor suministrado era insertado dentro de una función JavaScript,
      lo que abría la posibilidad de manipular el contexto de ejecución.
    </p>

    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal Result.png" alt="Reflexión en JavaScript">

    <h2>🛡️ Análisis del Filtro de Seguridad</h2>
    <p>
      La aplicación implementaba mecanismos de protección y un Web Application Firewall (WAF)
      orientado a bloquear patrones típicos de XSS.
    </p>

    <p>
      Sin embargo, el filtrado estaba basado en detección de palabras clave y patrones,
      no en codificación contextual adecuada.
    </p>

    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Script Tag.png" alt="Bloqueo del WAF">

    <h2>⚙️ Vector de Inyección</h2>
    <p>
      El parámetro vulnerable se encontraba dentro de un contexto JavaScript.
      Bajo ciertas condiciones, era posible romper el contexto original
      y ejecutar código arbitrario en el navegador del usuario.
    </p>

    <p>
      Se observaron comportamientos inconsistentes en el tratamiento de caracteres especiales
      y su representación codificada.
    </p>

    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Closing console.log.png" alt="Contexto JavaScript">

    <h2>🔐 Impacto Potencial</h2>
    <p>
      De haber sido explotada antes de su corrección, esta vulnerabilidad podría haber permitido:
    </p>

    <ul>
      <li>Ejecución de JavaScript arbitrario en el navegador de la víctima.</li>
      <li>Manipulación del DOM.</li>
      <li>Ataques de phishing dentro del mismo dominio.</li>
      <li>Acceso a información disponible en el contexto de sesión (según configuración de cookies).</li>
    </ul>

    <p>
      No se detectó evidencia de explotación activa.
    </p>

    <h2>🛠️ Remediación</h2>
    <p>
      Tras el reporte responsable a través del programa oficial,
      el equipo técnico implementó:
    </p>

    <ul>
      <li>Codificación contextual adecuada de la salida.</li>
      <li>Mejoras en la validación del lado servidor.</li>
      <li>Ajustes en las reglas del WAF.</li>
    </ul>

    <p>
      Posteriormente, la vulnerabilidad dejó de ser reproducible.
    </p>

    <h2>📜 Reconocimiento Oficial</h2>
    <div class="pdf-container">
      <iframe src="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0"
              width="100%" height="100%" style="border: none;"></iframe>
    </div>

    <div style="text-align: center; margin-top: 1rem;">
      <a href="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf"
         class="lang-btn" target="_blank">
        📥 Descargar Carta Oficial (PDF)
      </a>
    </div>

    <div class="collab-grid">
      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
          </svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.ivan.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.ivan.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.ivan.linkedin }}" target="_blank" rel="noopener noreferrer">
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>

      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
          </svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.diego.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.diego.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.diego.linkedin }}" target="_blank" rel="noopener noreferrer">
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
            <svg class="svg-icon" viewBox="0 0 24 24">
              <path d="M20 6L9 17l-5-5"></path>
            </svg>
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
            <svg class="svg-icon" viewBox="0 0 24 24">
              <path d="M12 9v4"></path>
              <path d="M12 17h.01"></path>
              <path d="M10.29 3.86l-8.12 14.06A2 2 0 0 0 3.9 21h16.2a2 2 0 0 0 1.73-3.08L13.71 3.86a2 2 0 0 0-3.42 0z"></path>
            </svg>
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
              <a href="{{ page.official_link }}" target="_blank">
                {{ page.official_link }}
              </a>
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
  if (e.key === 'Escape')
    document.getElementById('zoomOverlay').classList.remove('active');
});
</script>