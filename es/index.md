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
    <h1>Security Research Portfolio</h1>
    <p>Documentación profesional de vulnerabilidades e informes de bug bounty.</p>
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
          <th class="col-report">Reporte</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="col-date">2025-10-10</td>
          <td class="col-company">NFL</td>
          <td class="col-vuln">Reflected XSS in URL Search Query Parameter</td>
          <td class="col-severity"><span class="badge badge-p3">P3</span></td>
          <td class="col-status"><span class="status-icon" title="Resuelto">✅</span></td>
          <td class="col-report"><a href="./bounties/nfl-xss-search-query.md" class="icon-link" title="Ver Reporte">👁️</a></td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company">U. Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENCIAL]</code></td>
          <td class="col-severity"><span class="badge badge-p2">P2</span></td>
          <td class="col-status"><span class="status-icon" title="No Resuelto">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-18</td>
          <td class="col-company">US ITC</td>
          <td class="col-vuln"><code>[CONFIDENCIAL]</code></td>
          <td class="col-severity"><span class="badge badge-p3">P3</span></td>
          <td class="col-status"><span class="status-icon" title="No Resuelto">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-16</td>
          <td class="col-company">NASA JPL ECCO</td>
          <td class="col-vuln">Search Form XSS Cookie Exfiltration with WAF Bypass</td>
          <td class="col-severity"><span class="badge badge-p3">P3</span></td>
          <td class="col-status"><span class="status-icon" title="Resuelto">✅</span></td>
          <td class="col-report"><a href="./bounties/nasa-jpl-ecco-xss-waf-bypass.md" class="icon-link" title="Ver Reporte">👁️</a></td>
        </tr>
      </tbody>
    </table>
  </div>

  <hr>

  <div class="card-grid">
    <div class="glass-card">
      <h2 style="font-size: 1.25rem; margin-bottom: 2rem;">📊 Estadísticas</h2>
      <div class="card-item">
        <span>Empresas Impactadas</span>
        <strong>4 Corporaciones</strong>
      </div>
      <div class="card-item">
        <span>Informes Enviados</span>
        <strong>4 Bugs en total</strong>
      </div>
      <div class="card-item">
        <span>Tasa de Resolución</span>
        <strong>50.0% Éxito</strong>
      </div>
    </div>

    <div class="glass-card">
      <h2 style="font-size: 1.25rem; margin-bottom: 2rem;">🤝 Colaboración</h2>
      <div class="card-item">
        <span>NASA (ECCO JPL)</span>
        <strong>50% {{ site.researchers.ivan.name }} / 50% {{ site.researchers.diego.name }}</strong>
      </div>
      <div class="card-item">
        <span>NFL (HBCU Tournament)</span>
        <strong>50% {{ site.researchers.ivan.name }} / 50% {{ site.researchers.diego.name }}</strong>
      </div>
    </div>
  </div>

  <h2 class="section-title" style="margin-top: 60px;">🧠 Metodología</h2>
  <div class="glass-card">
    <ul class="method-list">
      <li><strong>Web App Sec</strong>: Descubrimiento de superficies de ataque ocultas.</li>
      <li><strong>Cultura de Bypass</strong>: Especialidad en evasión de XSS, WAF y filtrado de entrada.</li>
      <li><strong>Análisis Client-Side</strong>: Explotación avanzada de mecanismos del navegador.</li>
    </ul>
  </div>

  <footer>
    <p>Este portfolio se actualiza continuamente con nuevas investigaciones.</p>
  </footer>
</div>
