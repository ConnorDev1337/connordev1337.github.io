---
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ConnorDev | Security Research Portfolio</title>
    <link rel="stylesheet" href="../assets/css/styles.css">
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🛡️</text></svg>">
</head>
<body>

<div class="container">
  <div class="lang-switcher" style="padding-top: 20px;">
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
          <td class="col-status">
            <svg class="icon-svg icon-success" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nfl-xss-search-query/" class="icon-link" title="Open Report">
              <svg class="icon-svg icon-blue" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" /><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
              </svg>
            </a>
          </td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company">U. Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENTIAL RESEARCH]</code></td>
          <td class="col-severity"><span class="badge badge-p2">P2 / High</span></td>
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
          <td class="col-vuln"><code>[CONFIDENTIAL RESEARCH]</code></td>
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
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
          <td class="col-severity"><span class="badge badge-p3">P3 / Medium</span></td>
          <td class="col-status">
            <svg class="icon-svg icon-success" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nasa-jpl-ecco-xss-waf-bypass/" class="icon-link" title="Open Report">
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

  <h2 class="section-title">📊 Statistics</h2>
  <div class="card-grid">
    <div class="glass-card">
      <div class="card-item">
        <small>Impact Area</small>
        <strong>5 Corporate Networks</strong>
      </div>
      <div class="card-item">
        <small>Disclosed Vectors</small>
        <strong>4 Major Write-ups</strong>
      </div>
      <div class="card-item">
        <small>Remediation Rate</small>
        <strong>100% Verified Patching</strong>
      </div>
    </div>

    <div class="glass-card">
      <div class="card-item">
        <small>Lead Researcher (NASA/NFL)</small>
        <strong>{{ site.researchers.ivan.name }}</strong>
      </div>
      <div class="card-item">
        <small>Collaboration (JPL ECCO)</small>
        <strong>{{ site.researchers.ivan.name }} / {{ site.researchers.diego.name }}</strong>
      </div>
    </div>
  </div>

  <footer>
    <p>© 2026 ConnorDev | Research conducted under ethical disclosure guidelines.</p>
  </footer>
</div>

</body>
</html>