---
layout: default
submission_id: "4b01725e-288b-4db5-a38c-1a37f827d215"
official_link: "https://bugcrowd.com/submissions/4b01725e-288b-4db5-a38c-1a37f827d215"
---

<div class="container">
  <div class="lang-switcher">
    <a href="/en/bounties/nasa-2/" class="lang-btn">
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
    <h1>XSS Reflejado en Funcionalidad de Búsqueda con Técnicas Avanzadas de Bypass</h1>
    <p style="color: var(--p3); font-weight: 600;">
      National Aeronautics and Space Administration – Jet Propulsion Laboratory (NASA JPL)
    </p>
  </div>

  <div class="bounty-content">

    <h2>📌 Contexto</h2>
    <p>
      Durante la evaluación de seguridad de un subdominio público de NASA JPL,
      la funcionalidad de búsqueda fue analizada como parte de la revisión rutinaria
      del manejo de entrada de datos.
    </p>

    <p>
      La evaluación reveló que la entrada controlada por el usuario suministrada
      a través del parámetro de búsqueda se reflejaba dentro de un contexto
      JavaScript del lado del cliente sin codificación contextual adecuada.
    </p>

    <h2>🔍 Análisis Técnico</h2>
    <p>
      La evaluación de seguridad identificó que la funcionalidad de búsqueda
      reflejaba la entrada del usuario dentro de un contexto JavaScript sin
      codificación de salida adecuada.
    </p>

    <p>
      La vulnerabilidad permitía que entradas especialmente diseñadas pudieran
      romper el contexto JavaScript previsto y ejecutar código arbitrario en el navegador.
    </p>

    <h2>🛡️ Controles de Seguridad</h2>
    <p>
      La aplicación implementaba medidas defensivas incluyendo:
    </p>
    <ul>
      <li>Web Application Firewall (WAF) con detección de patrones XSS</li>
      <li>Filtrado de entrada para caracteres maliciosos</li>
      <li>Bloqueo basado en palabras clave de cadenas de ataque comunes</li>
    </ul>

    <p>
      Si bien estos controles demostraron protección robusta contra ataques básicos,
      técnicas de bypass avanzadas podrían eludir los mecanismos de filtrado.
    </p>

    <h2>🔐 Evaluación de Riesgos</h2>
    <p>
      La vulnerabilidad representaba un riesgo moderado debido a:
    </p>
    <ul>
      <li>Potencial de ejecución de JavaScript en navegadores víctima</li>
      <li>Capacidad de acceder a datos sensibles del lado del cliente</li>
      <li>Riesgo de ataques de phishing dentro del contexto de dominio confiable</li>
    </ul>

    <p>
      No se identificó evidencia de explotación maliciosa en el entorno real.
    </p>

    <h2>🔐 Impacto Potencial</h2>
    <p>
      Antes de la remediación, esta vulnerabilidad podría haber permitido:
    </p>

    <ul>
      <li>Ejecución de JavaScript arbitrario en navegadores víctima</li>
      <li>Acceso a datos sensibles del lado del cliente</li>
      <li>Secuestro de sesión dependiendo de la configuración de cookies</li>
      <li>Ataques de phishing dentro del contexto de dominio confiable</li>
    </ul>

    <p>
      No se observó evidencia de explotación en el mundo real.
    </p>

    <h2>🛠️ Remediación</h2>
    <p>
      El problema fue divulgado responsablemente a través del programa oficial
      de divulgación de vulnerabilidades.
    </p>

    <p>
      Después de la validación, la remediación incluyó:
    </p>

    <ul>
      <li>Codificación de salida consciente del contexto adecuada.</li>
      <li>Controles de validación del lado del servidor fortalecidos.</li>
      <li>Ajustes en el manejo de reglas del WAF.</li>
      <li>Mejora en la lógica de renderizado dentro del componente de búsqueda.</li>
    </ul>

    <p>
      Después de la remediación, el comportamiento vulnerable ya no fue reproducible.
    </p>

    <h2>📜 Reconocimiento Oficial</h2>
    <div class="pdf-container">
      <iframe src="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0"
              width="100%" height="100%" style="border: none;"></iframe>
    </div>

    <div style="text-align: center; margin-top: 1rem;">
      <a href="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf"
         class="lang-btn" target="_blank">
        📥 Descargar Reconocimiento Oficial (PDF)
      </a>
    </div>

    <h2>📚 Lecciones de Seguridad</h2>
    <ul>
      <li>La codificación de salida consciente del contexto es crítica para la prevención de XSS</li>
      <li>El filtrado basado en patrones por sí solo proporciona protección insuficiente</li>
      <li>El WAF debe complementar, no reemplazar, las prácticas de codificación segura</li>
      <li>La funcionalidad de cara al público requiere revisión de seguridad exhaustiva</li>
      <li>El enfoque de defensa en profundidad es esencial para la seguridad web</li>
    </ul>

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
          <div class="meta-title">
            <strong>Estado</strong>
          </div>
        </div>
        <div class="meta-badge success">Resuelto</div>
      </div>

      <div class="meta-card">
        <div class="meta-left">
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
          <small>ID de Envío</small>
          <div class="value">{{ page.submission_id | default: 'N/A' }}</div>
        </div>
        <div class="verification-item">
          <small>Enlace Oficial</small>
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
