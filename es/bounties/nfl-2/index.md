---
layout: default
submission_id: "e8471382-6e56-4e74-b4e2-ff20279371ff"
official_link: "https://bugcrowd.com/submissions/e8471382-6e56-4e74-b4e2-ff20279371ff"
---

<div class="container">
  <div class="lang-switcher">
    <a href="/en/bounties/nfl-xss-search-query/" class="lang-btn">
      <span class="label-full">English (EN)</span>
      <span class="label-short">EN</span>
    </a>
    <a href="./" class="lang-btn active">
      <span class="label-full">Español (ES)</span>
      <span class="label-short">ES</span>
    </a>
  </div>

<article>
  <a href="/es/" class="back-btn">
    <svg viewBox="0 0 24 24">
      <polyline points="15 18 9 12 15 6"></polyline>
    </svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>XSS Reflejado en Funcionalidad de Búsqueda</h1>
    <p style="color: var(--p3); font-weight: 600;">
      National Football League (NFL)
    </p>
  </div>

  <div class="bounty-content">

    <h2>📌 Contexto</h2>
    <p>
      Durante la evaluación de seguridad del subdominio público 
      <code>hbcutournament.nfl.com</code>, 
      se analizó el comportamiento de la funcionalidad de búsqueda disponible para usuarios no autenticados.
    </p>

    <p>
      El análisis reveló que el parámetro de búsqueda en la URL reflejaba directamente 
      la entrada suministrada por el usuario dentro del contenido renderizado, 
      sin aplicar una codificación contextual adecuada.
    </p>

    <h2>🔎 Identificación Técnica</h2>
    <p>
      La vulnerabilidad se encontraba en el parámetro:
    </p>

    <pre><code>https://hbcutournament.nfl.com/{resources,blogs}?q=</code></pre>

    <p>
      El valor del parámetro <code>q</code> era incorporado en la respuesta HTML 
      sin sanitización suficiente, permitiendo la interpretación de contenido 
      controlado por el usuario como parte del DOM.
    </p>

    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - HTML Injection.png" alt="Reflexión de Entrada">

    <p>
      Las pruebas realizadas confirmaron que el navegador procesaba 
      determinadas estructuras HTML inyectadas a través del parámetro.
    </p>

    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Alert 1.png" alt="Ejecución en el Navegador">

    <h2>⚙️ Análisis del Comportamiento</h2>
    <p>
      El flujo de renderizado no implementaba una estrategia de codificación 
      basada en contexto (HTML, atributo, JavaScript).
    </p>

    <p>
      Esto permitía que contenido especialmente construido fuera interpretado 
      por el navegador como código ejecutable, en lugar de tratarse como texto plano.
    </p>

    <p>
      No se requería autenticación para explotar el comportamiento observado.
    </p>

    <h2>🔐 Impacto Potencial</h2>
    <p>
      Antes de su corrección, la vulnerabilidad podía permitir:
    </p>

    <ul>
      <li>Ejecución de JavaScript arbitrario en el navegador de la víctima.</li>
      <li>Manipulación del contenido de la página dentro del dominio legítimo.</li>
      <li>Creación de escenarios de phishing utilizando confianza en la marca.</li>
      <li>Acceso a información accesible desde el contexto del navegador, dependiendo de la configuración de seguridad.</li>
    </ul>

    <p>
      No se identificaron indicios de explotación activa en producción.
    </p>

    <h2>🛠️ Proceso de Remediación</h2>
    <p>
      La vulnerabilidad fue reportada de manera responsable a través 
      del programa oficial de recompensas gestionado en Bugcrowd.
    </p>

    <p>
      Tras la validación del hallazgo, el equipo técnico implementó:
    </p>

    <ul>
      <li>Codificación contextual adecuada del parámetro reflejado.</li>
      <li>Refuerzo de validación del lado servidor.</li>
      <li>Ajustes en el manejo del renderizado dinámico del buscador.</li>
    </ul>

    <p>
      Posteriormente, el comportamiento vulnerable dejó de ser reproducible.
    </p>

    <h2>📚 Consideraciones Técnicas</h2>
    <p>
      Este caso refuerza varios principios fundamentales:
    </p>

    <ul>
      <li>Los parámetros de búsqueda son vectores comunes de inyección.</li>
      <li>El filtrado basado únicamente en listas negras es insuficiente.</li>
      <li>La codificación contextual es esencial para prevenir vulnerabilidades XSS.</li>
      <li>Las funcionalidades públicas deben considerarse superficies de ataque activas.</li>
    </ul>

    <h2>🤝 Detalle de Colaboración</h2>
    <p>
      Esta investigación fue realizada en colaboración con los siguientes investigadores:
    </p>

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
              <svg class="svg-icon" viewBox="0 0 24 24">
                <path d="M12 2l7 4v6c0 5-3 9-7 10C8 21 5 17 5 12V6z"></path>
                <path d="M9 12l2 2 4-5"></path>
              </svg>
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.ivan.linkedin }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24">
                <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4V9h4v2"></path>
                <rect x="2" y="9" width="4" height="12"></rect>
                <circle cx="4" cy="4" r="2"></circle>
              </svg>
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
              <svg class="svg-icon" viewBox="0 0 24 24">
                <path d="M12 2l7 4v6c0 5-3 9-7 10C8 21 5 17 5 12V6z"></path>
                <path d="M9 12l2 2 4-5"></path>
              </svg>
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.diego.linkedin }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24">
                <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4V9h4v2"></path>
                <rect x="2" y="9" width="4" height="12"></rect>
                <circle cx="4" cy="4" r="2"></circle>
              </svg>
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
              <a href="{{ page.official_link }}" target="_blank" rel="noopener noreferrer">
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