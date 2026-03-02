---
layout: default
title: ConnorDev | Security Research Portfolio
---

<div class="lang-switcher">
  <a href="../es/" class="lang-btn">Español (ES)</a>
</div>

<header>
  <h1>Bug Bounty Portfolio</h1>
  <p style="color: var(--text-secondary); font-size: 1.1rem;">Security Research & Detailed Vulnerability Reports.</p>
</header>

<h2 class="section-title">📅 Timeline</h2>

<table>
  <thead>
    <tr>
      <th>Date</th>
      <th>Company</th>
      <th>Vulnerability</th>
      <th>Severity</th>
      <th>Status</th>
      <th>Report</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2025-10-10</td>
      <td>NFL</td>
      <td>Reflected XSS in URL Search Query Parameter</td>
      <td style="text-align:center;"><span class="badge badge-p3">P3</span></td>
      <td style="text-align:center;"><span class="status-icon" title="Resolved">✅</span></td>
      <td style="text-align:center;"><a href="./bounties/nfl-xss-search-query.md" class="view-link" title="View Report">👁️</a></td>
    </tr>
    <tr>
      <td>2025-09-25</td>
      <td>University of Canterbury</td>
      <td><code>[CONFIDENTIAL]</code></td>
      <td style="text-align:center;"><span class="badge badge-p2">P2</span></td>
      <td style="text-align:center;"><span class="status-icon" title="Unresolved">⏳</span></td>
      <td style="text-align:center;">-</td>
    </tr>
    <tr>
      <td>2025-09-18</td>
      <td>US International Trade Commission</td>
      <td><code>[CONFIDENTIAL]</code></td>
      <td style="text-align:center;"><span class="badge badge-p3">P3</span></td>
      <td style="text-align:center;"><span class="status-icon" title="Unresolved">⏳</span></td>
      <td style="text-align:center;">-</td>
    </tr>
    <tr>
      <td>2025-09-16</td>
      <td>NASA JPL ECCO</td>
      <td>Search Form XSS Cookie Exfiltration with WAF Bypass</td>
      <td style="text-align:center;"><span class="badge badge-p3">P3</span></td>
      <td style="text-align:center;"><span class="status-icon" title="Resolved">✅</span></td>
      <td style="text-align:center;"><a href="./bounties/nasa-jpl-ecco-xss-waf-bypass.md" class="view-link" title="View Report">👁️</a></td>
    </tr>
  </tbody>
</table>

---

<div class="summary-container">
  <h2 class="section-title">📊 Summary</h2>
  <div class="summary-item">Impacted Companies: <strong>4</strong></div>
  <div class="summary-item">Vulnerabilities Reported: <strong>4</strong></div>
  <div class="summary-item">Resolved Status: <strong>2/4</strong></div>
  
  <h3 style="margin-top:2rem; font-size:1.1rem; color:var(--text-secondary);">🤝 Collaborative Research</h3>
  <div class="summary-item"><strong>NASA (ECCO JPL)</strong>: 50% {{ site.researchers.ivan.name }} / 50% {{ site.researchers.diego.name }}</div>
  <div class="summary-item"><strong>NFL (HBCU Tournament)</strong>: 50% {{ site.researchers.ivan.name }} / 50% {{ site.researchers.diego.name }}</div>
</div>

---

<h2 class="section-title">🧠 Methodology Focus</h2>

<ul style="list-style:none; padding:0;">
  <li class="summary-item"><strong>Web Application Security</strong>: Discovering hidden attack surfaces.</li>
  <li class="summary-item"><strong>XSS & WAF Bypass</strong>: Deep analysis of input filtering & encoding bypass techniques.</li>
  <li class="summary-item"><strong>Client-Side Attacks</strong>: Analyzing modern browser-side security mechanisms.</li>
</ul>

<p style="text-align:center; color:var(--text-secondary); margin-top:4rem; font-style:italic;">This portfolio is continuously updated.</p>