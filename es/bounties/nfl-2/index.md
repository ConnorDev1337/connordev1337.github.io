---
layout: default
submission_id: "e8471382-6e56-4e74-b4e2-ff20279371ff"
official_link: "https://bugcrowd.com/submissions/e8471382-6e56-4e74-b4e2-ff20279371ff"
---

<div class="container">
  <div class="lang-switcher">
    <a href="/en/bounties/nfl-2/" class="lang-btn">
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
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>XSS Reflejado en Buscador con Bypass de Filtros (Caso de Estudio)</h1>
    <p style="color: var(--p3); font-weight: 600;">
      National Football League (NFL)
    </p>
  </div>

  <div class="bounty-content">

    <h2>📌 Contexto</h2>
    <p>
      Durante el análisis de seguridad del subdominio público <code>hbcutournament.nfl.com</code>,
      se evaluó la funcionalidad de búsqueda accesible sin autenticación.
    </p>

    <p>
      El objetivo era revisar el tratamiento de entradas controladas por el usuario
      dentro del flujo de renderizado dinámico del lado cliente.
    </p>

    <h2>🔍 Identificación Inicial</h2>
    <p>
      Se observó que el parámetro de búsqueda reflejaba directamente el valor
      suministrado por el usuario dentro de la respuesta generada.
    </p>

    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - HTML Injection.png" alt="Búsqueda normal">

    <p>
      Al inspeccionar el código fuente, se detectó que la entrada era insertada
      directamente dentro del HTML renderizado sin codificación contextual.
    </p>

    <p>
      Este comportamiento indicaba un posible escenario de ejecución no intencionada
      si el contexto de salida no era correctamente protegido.
    </p>

    <h2>⚙️ Análisis Técnico</h2>
    <p>
      La vulnerabilidad se encontraba en el parámetro URL:
    </p>

    <pre><code>https://hbcutournament.nfl.com/{resources,blogs}?q=</code></pre>

    <p>
      El valor del parámetro <code>q</code> era incorporado en la respuesta HTML
      sin sanitización suficiente, permitiendo la interpretación de contenido
      controlado por el usuario como parte del DOM.
    </p>

    <p>
      Durante la investigación se comprobó que ciertas construcciones HTML
      y JavaScript permitían evadir los mecanismos de filtrado implementados.
    </p>

    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Alert 1.png" alt="Ejecución en el navegador">

    <p>
      Este comportamiento confirmaba la existencia de un escenario
      de Cross-Site Scripting reflejado en contexto no seguro.
    </p>

    <h2>🔐 Impacto Potencial</h2>
    <p>
      Antes de su corrección, esta vulnerabilidad podía permitir:
    </p>

    <ul>
      <li>Ejecución de JavaScript arbitrario en el navegador de la víctima.</li>
      <li>Manipulación del contenido de la página dentro del dominio legítimo.</li>
      <li>Creación de escenarios de phishing utilizando confianza en la marca.</li>
      <li>Acceso a información accesible desde el contexto del navegador, dependiendo de la configuración de seguridad.</li>
    </ul>

    <p>
      La explotación requería interacción del usuario mediante un enlace especialmente construido.
    </p>

    <h2>🚫 Sobre la Exfiltración</h2>
    <p>
      Durante la fase de prueba controlada se evaluó la posibilidad
      de acceder a información disponible en el contexto del navegador,
      demostrando el impacto potencial del hallazgo.
    </p>

    <p>
      Los detalles técnicos específicos han sido omitidos deliberadamente
      en esta publicación como parte de una divulgación responsable.
    </p>

    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Cookies Exfiltration Result.png" alt="Demostración controlada">

    <h2>🛠️ Proceso de Remediación</h2>
    <p>
      La vulnerabilidad fue reportada a través del programa oficial
      de recompensas gestionado en Bugcrowd.
    </p>

    <p>
      Tras la validación, el equipo técnico aplicó:
    </p>

    <ul>
      <li>Codificación contextual adecuada del parámetro reflejado.</li>
      <li>Refuerzo de validación del lado servidor.</li>
      <li>Ajustes en el manejo del renderizado dinámico del buscador.</li>
      <li>Mejoras en el tratamiento de entradas dinámicas.</li>
    </ul>

    <p>
      Posteriormente, el comportamiento vulnerable dejó de ser reproducible.
    </p>

    <h2>📚 Lecciones Técnicas</h2>
    <ul>
      <li>Los parámetros de búsqueda son vectores comunes de inyección.</li>
      <li>El filtrado basado únicamente en listas negras es insuficiente.</li>
      <li>La codificación contextual es esencial para prevenir vulnerabilidades XSS.</li>
      <li>Las funcionalidades públicas deben considerarse superficies de ataque activas.</li>
    </ul>

    <h2>📜 Responsible Disclosure Notice</h2>
    <p>
      Esta vulnerabilidad fue reportada y resuelta conforme al
      NFL Vulnerability Disclosure Program.
    </p>
    <p>
      Los detalles técnicos explotables han sido omitidos intencionadamente.
      La divulgación se realiza tras la corrección confirmada del hallazgo.
    </p>

    <h2>📜 Reconocimiento Oficial</h2>
    <div class="pdf-container">
      <iframe src="/assets/others/nfl-xss-search-query/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0"
              width="100%" height="100%" style="border: none;"></iframe>
    </div>

    <div style="text-align: center; margin-top: 1rem;">
      <a href="/assets/others/nfl-xss-search-query/acknowledgment.pdf"
         class="lang-btn" target="_blank">
        📥 Descargar Carta Oficial (PDF)
      </a>
    </div>

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
          <div class="meta-title"><strong>Estado</strong></div>
        </div>
        <div class="meta-badge success">Resuelto</div>
      </div>

      <div class="meta-card">
        <div class="meta-left">
          <div class="meta-title"><strong>Severidad</strong></div>
        </div>
        <div class="meta-badge medium">P3</div>
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
