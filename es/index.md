---
layout: default
title: ConnorDev | Security Research Portfolio
---

<div class="container">
  <div class="lang-switcher" style="padding-top: 20px;">
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
          <td class="col-status">
            <svg class="icon-svg icon-success" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nfl-xss-search-query/" class="icon-link" title="Ver Reporte">
              <svg class="icon-svg icon-blue" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" /><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
              </svg>
            </a>
          </td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company">U. Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENCIAL]</code></td>
          <td class="col-severity"><span class="badge badge-p2">P2 / Alta</span></td>
          <td class="col-status">
            <svg class="icon-svg icon-pending" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6h4.5m4.5 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
          </td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-18</td>
          <td class="col-company">US ITC</td>
          <td class="col-vuln"><code>[CONFIDENCIAL]</code></td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Media</span></td>
          <td class="col-status">
            <svg class="icon-svg icon-pending" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6h4.5m4.5 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
          </td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-16</td>
          <td class="col-company">NASA JPL ECCO</td>
          <td class="col-vuln">Search Form XSS Cookie Exfiltration with WAF Bypass</td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Media</span></td>
          <td class="col-status">
            <svg class="icon-svg icon-success" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nasa-jpl-ecco-xss-waf-bypass/" class="icon-link" title="Ver Reporte">
              <svg class="icon-svg icon-blue" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" /><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
              </svg>
            </a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <h2 class="section-title">🏆 Wall of Fame</h2>
  <div class="wof-container">
    <a href="https://bugcrowd.com/engagements/nasa-vdp/hall_of_fames" target="_blank" class="wof-logo">
      <img src="https://logos.bugcrowdusercontent.com/logos/e34e/ced8/462922dd/fe2d0bf28f7b56095470ca07d0421e95_1615217311496.jpeg" alt="NASA">
    </a>
    <a href="https://bugcrowd.com/engagements/nfl-vdp-pro/hall_of_fames" target="_blank" class="wof-logo">
      <img src="https://logos.bugcrowdusercontent.com/logos/dc8e/976c/23d576ba/cbf9d3c4e7dc5a974e27565bad46ae76_national_football_league_logo.jpeg" alt="NFL">
    </a>
    <a href="https://bugcrowd.com/engagements/university-canterbury-vdp-pro/hall_of_fames" target="_blank" class="wof-logo">
      <img src="https://logos.bugcrowdusercontent.com/logos/3f7e/826a/d0f78ced/a58f55915330f1e5d19dc75e462129d6_university_of_canterbury_logo.jpeg" alt="University of Canterbury">
    </a>
    <a href="https://bugcrowd.com/engagements/gamingcorps-vdp-pro/hall_of_fames" target="_blank" class="wof-logo">
      <img src="https://logos.bugcrowdusercontent.com/logos/3e27/2dae/d8b0a7b5/a44c8386bc03fa99c885f02a925f0d81_gaming_corps_logo.jpeg" alt="Gaming Corps">
    </a>
    <a href="https://bugcrowd.com/engagements/usitc-vdp/hall_of_fames" target="_blank" class="wof-logo">
      <img src="https://logos.bugcrowdusercontent.com/logos/df14/f8f6/e5217321/d6766f0d7e4864eca4d059e018e16bb7_U.S._International_Trade_Commission.png" alt="US ITC">
    </a>
  </div>

  <h2 class="section-title">📊 Estadísticas</h2>
  <div class="card-grid">
    <div class="glass-card">
      <div class="card-item">
        <small>Alcance de Impacto</small>
        <strong>5 Corporaciones</strong>
      </div>
      <div class="card-item">
        <small>Vectores Revelados</small>
        <strong>4 Write-ups</strong>
      </div>
      <div class="card-item">
        <small>Tasa de Resolución</small>
        <strong>100% Verificado</strong>
      </div>
    </div>

    <div class="glass-card">
      <div class="card-item">
        <small>Investigador Principal</small>
        <strong>{{ site.researchers.ivan.name }}</strong>
      </div>
      <div class="card-item">
        <small>Colaboración (JPL ECCO)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
    </div>
  </div>

  <footer>
    <p>© 2026 ConnorDev | Investigación bajo pautas de divulgación ética.</p>
  </footer>
</div>
