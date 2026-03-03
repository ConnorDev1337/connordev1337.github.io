---
layout: default
title: ConnorDev | Security Research Portfolio
---

<div class="container">
  <div class="lang-switcher">
    <a href="../en/" class="lang-btn">English (EN)</a>
    <a href="./" class="lang-btn active">Español (ES)</a>
  </div>

  <header>
    <h1>Cyber Research Portfolio</h1>
    <p>Documentación científica de vulnerabilidades reportadas, técnicas de bypass e investigación de seguridad.</p>
  </header>

  <h2 class="section-title">📅 Cronología</h2>
  
  <div class="board-wrapper">
    <table>
      <thead>
        <tr>
          <th class="col-date">Fecha</th>
          <th class="col-company">Empresa</th>
          <th class="col-vuln">Vulnerabilidad</th>
          <th class="col-severity">Severidad</th>
          <th class="col-status">Estado</th>
          <th class="col-report">Acceder</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="col-date">2025-10-10</td>
          <td class="col-company">NFL</td>
          <td class="col-vuln">Reflected XSS in URL Search Query Parameter</td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Media</span></td>
          <td class="col-status"><span class="status-icon" title="Resuelto">✅</span></td>
          <td class="col-report"><a href="./bounties/nfl-xss-search-query/" class="icon-link" title="Ver Reporte">👁️</a></td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company">U. Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENCIAL / INVESTIGACIÓN]</code></td>
          <td class="col-severity"><span class="badge badge-p2">P2 / Alta</span></td>
          <td class="col-status"><span class="status-icon" title="Esperando Resolución">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-18</td>
          <td class="col-company">US ITC</td>
          <td class="col-vuln"><code>[CONFIDENCIAL / INVESTIGACIÓN]</code></td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Media</span></td>
          <td class="col-status"><span class="status-icon" title="Esperando Resolución">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-16</td>
          <td class="col-company">NASA JPL ECCO</td>
          <td class="col-vuln">Search Form XSS Cookie Exfiltration with WAF Bypass</td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Media</span></td>
          <td class="col-status"><span class="status-icon" title="Resuelto">✅</span></td>
          <td class="col-report"><a href="./bounties/nasa-jpl-ecco-xss-waf-bypass/" class="icon-link" title="Ver Reporte">👁️</a></td>
        </tr>
      </tbody>
    </table>
  </div>

  <hr>

  <div class="card-grid">
    <div class="glass-card">
      <h2 style="font-size: 1.15rem; margin-bottom: 2rem; color: var(--text-secondary);">📊 Estadísticas</h2>
      <div class="card-item">
        <small>Alcance de Impacto</small>
        <strong>4 Corporaciones</strong>
      </div>
      <div class="card-item">
        <small>Vectores Revelados</small>
        <strong>4 Write-ups</strong>
      </div>
      <div class="card-item">
        <small>Tasa de Resolución</small>
        <strong>50.0% Verificado</strong>
      </div>
    </div>

    <div class="glass-card">
      <h2 style="font-size: 1.15rem; margin-bottom: 2rem; color: var(--text-secondary);">🤝 Colaboración</h2>
      <div class="card-item">
        <small>Investigación Conjunta (NASA)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
      <div class="card-item">
        <small>Investigación Conjunta (NFL)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
    </div>
  </div>

  <h2 class="section-title" style="margin-top: 60px;">🧠 Framework de Investigación</h2>
  <div class="glass-card method-card">
    <ul class="method-list">
      <li><strong>Fase de Descubrimiento</strong>: Mapeo exhaustivo de endpoints y lógica de aplicación.</li>
      <li><strong>Explotación & Bypass</strong>: Supervisión de filtros con payloads avanzados para evadir WAF.</li>
      <li><strong>Análisis de Impacto</strong>: Verificación de vectores de exfiltración (XSS, Session Takeover).</li>
    </ul>
  </div>

  <footer>
    <p>© 2026 ConnorDev | Investigación bajo pautas de divulgación ética.</p>
  </footer>
</div>
