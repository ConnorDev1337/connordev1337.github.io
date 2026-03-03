---
layout: default
title: ConnorDev | Security Research Portfolio
---

<div class="container">
  <div class="lang-switcher">
    <a href="../es/" class="lang-btn">Español (ES)</a>
    <a href="./" class="lang-btn active">English (EN)</a>
  </div>

  <header>
    <h1>{{ site.title }}</h1>
    <p style="color: var(--text-secondary); font-size: 1.1rem; max-width: 800px;">
      {{ site.description_en }}
    </p>
  </header>

  <h2 class="section-title">Timeline</h2>
  <div class="board-wrapper">
    <table>
      <thead>
        <tr>
          <th class="col-date">Date</th>
          <th class="col-company">Company</th>
          <th class="col-vuln">Vulnerability</th>
          <th class="col-severity">Severity</th>
          <th class="col-status">Status</th>
          <th class="col-report">Access</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="col-date">2025-10-10</td>
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/dc8e/976c/23d576ba/cbf9d3c4e7dc5a974e27565bad46ae76_national_football_league_logo.jpeg" alt="NFL">NFL</td>
          <td class="col-vuln">Reflected XSS in URL Search Query Parameter</td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-success" viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nfl-xss-search-query/" class="icon-link" title="Open Report">
              <svg class="svg-icon icon-view" viewBox="0 0 24 24"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path><circle cx="12" cy="12" r="3"></circle></svg>
            </a>
          </td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/3f7e/826a/d0f78ced/a58f55915330f1e5d19dc75e462129d6_university_of_canterbury_logo.jpeg" alt="UC">University of Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENTIAL RESEARCH]</code></td>
          <td class="col-severity"><span class="badge badge-p2">P2 / High</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-pending" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>
          </td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-18</td>
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/df14/f8f6/e5217321/d6766f0d7e4864eca4d059e018e16bb7_U.S._International_Trade_Commission.png" alt="USITC">US ITC</td>
          <td class="col-vuln"><code>[CONFIDENTIAL RESEARCH]</code></td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-pending" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>
          </td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-16</td>
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/e34e/ced8/462922dd/fe2d0bf28f7b56095470ca07d0421e95_1615217311496.jpeg" alt="NASA">NASA JPL ECCO</td>
          <td class="col-vuln">Search Form XSS Cookie Exfiltration with WAF Bypass</td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-success" viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nasa-jpl-ecco-xss-waf-bypass/" class="icon-link" title="Open Report">
              <svg class="svg-icon icon-view" viewBox="0 0 24 24"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path><circle cx="12" cy="12" r="3"></circle></svg>
            </a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="card-grid">
    <div class="glass-card">
      <h2 style="font-size: 1.15rem; margin-bottom: 2rem; color: var(--text-secondary);">📊 Stats Dashboard</h2>
      <div class="card-item">
        <small>Impact Scope</small>
        <strong>5 Global Entities</strong>
      </div>
      <div class="card-item">
        <small>Research Vectors</small>
        <strong>4 Write-ups</strong>
      </div>
      <div class="card-item">
        <small>Resolution Rate</small>
        <strong>100% Verified</strong>
      </div>
    </div>

    <div class="glass-card">
      <h2 style="font-size: 1.15rem; margin-bottom: 2rem; color: var(--text-secondary);">🤝 Collaboration</h2>
      <div class="card-item">
        <small>Joint Research (NASA)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
      <div class="card-item">
        <small>Joint Research (NFL)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
    </div>
  </div>

  <h2 class="section-title section-mt">Wall of Fame</h2>
  <div class="wof-grid">
    <a href="https://bugcrowd.com/engagements/nfl-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="NFL Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/dc8e/976c/23d576ba/cbf9d3c4e7dc5a974e27565bad46ae76_national_football_league_logo.jpeg" alt="NFL">
    </a>
    <a href="https://bugcrowd.com/engagements/university-of-canterbury-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="University of Canterbury Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/3f7e/826a/d0f78ced/a58f55915330f1e5d19dc75e462129d6_university_of_canterbury_logo.jpeg" alt="University of Canterbury">
    </a>
    <a href="https://bugcrowd.com/engagements/usitc-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="US International Trade Commission Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/df14/f8f6/e5217321/d6766f0d7e4864eca4d059e018e16bb7_U.S._International_Trade_Commission.png" alt="US ITC">
    </a>
    <a href="https://bugcrowd.com/engagements/nasa-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="NASA Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/e34e/ced8/462922dd/fe2d0bf28f7b56095470ca07d0421e95_1615217311496.jpeg" alt="NASA">
    </a>
    <a href="https://bugcrowd.com/engagements/gamingcorps-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="Gaming Corps Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/3e27/2dae/d8b0a7b5/a44c8386bc03fa99c885f02a925f0d81_gaming_corps_logo.jpeg" alt="Gaming Corps">
    </a>
  </div>

  <footer>
    <p>© 2026 ConnorDev | Research conducted under ethical disclosure guidelines.</p>
  </footer>
</div>