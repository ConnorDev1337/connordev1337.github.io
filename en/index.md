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
    <h1>Security Research Portfolio</h1>
    <p>Professional documentation of vulnerabilities and bug bounty reports.</p>
  </header>

  <h2 class="section-title">📅 Timeline</h2>
  
  <div class="board-wrapper">
    <table>
      <thead>
        <tr>
          <th class="col-date">Date</th>
          <th class="col-company">Company</th>
          <th class="col-vuln">Vulnerability</th>
          <th class="col-severity">Severity</th>
          <th class="col-status">Status</th>
          <th class="col-report">Report</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="col-date">2025-10-10</td>
          <td class="col-company">NFL</td>
          <td class="col-vuln">Reflected XSS in URL Search Query Parameter</td>
          <td class="col-severity"><span class="badge badge-p3">P3</span></td>
          <td class="col-status"><span class="status-icon" title="Resolved">✅</span></td>
          <td class="col-report"><a href="./bounties/nfl-xss-search-query.md" class="icon-link" title="View Report">👁️</a></td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company">U. Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENTIAL]</code></td>
          <td class="col-severity"><span class="badge badge-p2">P2</span></td>
          <td class="col-status"><span class="status-icon" title="Unresolved">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-18</td>
          <td class="col-company">US ITC</td>
          <td class="col-vuln"><code>[CONFIDENTIAL]</code></td>
          <td class="col-severity"><span class="badge badge-p3">P3</span></td>
          <td class="col-status"><span class="status-icon" title="Unresolved">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-16</td>
          <td class="col-company">NASA JPL ECCO</td>
          <td class="col-vuln">Search Form XSS Cookie Exfiltration with WAF Bypass</td>
          <td class="col-severity"><span class="badge badge-p3">P3</span></td>
          <td class="col-status"><span class="status-icon" title="Resolved">✅</span></td>
          <td class="col-report"><a href="./bounties/nasa-jpl-ecco-xss-waf-bypass.md" class="icon-link" title="View Report">👁️</a></td>
        </tr>
      </tbody>
    </table>
  </div>

  <hr>

  <div class="card-grid">
    <div class="glass-card">
      <h2 style="font-size: 1.25rem; margin-bottom: 2rem;">📊 Statistics</h2>
      <div class="card-item">
        <span>Impacted Companies</span>
        <strong>4 Corporations</strong>
      </div>
      <div class="card-item">
        <span>Reports Sent</span>
        <strong>4 Total Bugs</strong>
      </div>
      <div class="card-item">
        <span>Resolution Rate</span>
        <strong>50.0% Success</strong>
      </div>
    </div>

    <div class="glass-card">
      <h2 style="font-size: 1.25rem; margin-bottom: 2rem;">🤝 Collaboration</h2>
      <div class="card-item">
        <span>NASA (ECCO JPL)</span>
        <strong>50% {{ site.researchers.ivan.name }} / 50% {{ site.researchers.diego.name }}</strong>
      </div>
      <div class="card-item">
        <span>NFL (HBCU)</span>
        <strong>50% {{ site.researchers.ivan.name }} / 50% {{ site.researchers.diego.name }}</strong>
      </div>
    </div>
  </div>

  <h2 class="section-title" style="margin-top: 60px;">🧠 Methodology</h2>
  <div class="glass-card">
    <ul class="method-list">
      <li><strong>Web App Sec</strong>: Discovery of hidden attack surfaces & logic flows.</li>
      <li><strong>Bypass Culture</strong>: Expertise in XSS, WAF & input filtering evasion.</li>
      <li><strong>Client-Side Analysis</strong>: Advanced exploitation of browser-side security mechanisms.</li>
    </ul>
  </div>

  <footer>
    <p>This portfolio is continuously updated with new research.</p>
  </footer>
</div>