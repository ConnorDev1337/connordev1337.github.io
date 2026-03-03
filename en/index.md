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
    <h1>Cyber Research Portfolio</h1>
    <p>Scientific documentation of disclosed vulnerabilities, bypass techniques, and security research.</p>
  </header>

  <h2 class="section-title">📅 Timeline</h2>
  
  <div class="board-wrapper">
    <table>
      <thead>
        <tr>
          <th class="col-date">Date</th>
          <th class="col-company">Company</th>
          <th class="col-vuln">Vulnerability Research</th>
          <th class="col-severity">Severity</th>
          <th class="col-status">Status</th>
          <th class="col-report">Access</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="col-date">2025-10-10</td>
          <td class="col-company">NFL</td>
          <td class="col-vuln">Reflected XSS in URL Search Query Parameter</td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
          <td class="col-status"><span class="status-icon" title="Resolved">✅</span></td>
          <td class="col-report"><a href="./bounties/nfl-xss-search-query/" class="icon-link" title="Open Report">👁️</a></td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company">U. Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENTIAL RESEARCH]</code></td>
          <td class="col-severity"><span class="badge badge-p2">P2 / High</span></td>
          <td class="col-status"><span class="status-icon" title="Awaiting Resolution">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-18</td>
          <td class="col-company">US ITC</td>
          <td class="col-vuln"><code>[CONFIDENTIAL RESEARCH]</code></td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
          <td class="col-status"><span class="status-icon" title="Awaiting Resolution">⏳</span></td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-16</td>
          <td class="col-company">NASA JPL ECCO</td>
          <td class="col-vuln">Search Form XSS Cookie Exfiltration with WAF Bypass</td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
          <td class="col-status"><span class="status-icon" title="Resolved">✅</span></td>
          <td class="col-report"><a href="./bounties/nasa-jpl-ecco-xss-waf-bypass/" class="icon-link" title="Open Report">👁️</a></td>
        </tr>
      </tbody>
    </table>
  </div>

  <hr>

  <div class="card-grid">
    <div class="glass-card">
      <h2 style="font-size: 1.15rem; margin-bottom: 2rem; color: var(--text-secondary);">📊 Stats Dashboard</h2>
      <div class="card-item">
        <small>Impact Area</small>
        <strong>4 Major Entities</strong>
      </div>
      <div class="card-item">
        <small>Disclosed Vectors</small>
        <strong>4 Security Write-ups</strong>
      </div>
      <div class="card-item">
        <small>Remediation Rate</small>
        <strong>50.0% Verified</strong>
      </div>
    </div>

    <div class="glass-card">
      <h2 style="font-size: 1.15rem; margin-bottom: 2rem; color: var(--text-secondary);">🤝 Team Collaboration</h2>
      <div class="card-item">
        <small>Joint Research (NASA JPL)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
      <div class="card-item">
        <small>Joint Research (NFL)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
    </div>
  </div>

  <h2 class="section-title" style="margin-top: 60px;">🧠 Research Framework</h2>
  <div class="glass-card method-card">
    <ul class="method-list">
      <li><strong>Discovery Phase</strong>: Deep mapping of hidden application logic and API endpoints.</li>
      <li><strong>Exploitation & Bypass</strong>: Advancing beyond filtering with WAF-evading payloads.</li>
      <li><strong>Impact Analysis</strong>: Verifying critical data exfiltration vectors (XSS, Session Takeover).</li>
    </ul>
  </div>

  <footer>
    <p>© 2026 ConnorDev | Research conducted under ethical disclosure guidelines.</p>
  </footer>
</div>