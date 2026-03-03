---
layout: default
title: ConnorDev | Bug Bounty Portfolio
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

  <h2 class="section-title section-mt">Statistics</h2>
  <div class="card-grid">
    <div class="glass-card stat-card">
      <div class="stat-top">
        <div class="stat-meta">
          <div class="stat-icon">
            <svg class="svg-icon" viewBox="0 0 24 24"><path d="M3 3v18h18"></path><path d="M7 14l3-3 3 3 6-6"></path></svg>
          </div>
          <div class="stat-title">
            <small>Impact Scope</small>
            <div class="stat-value">5</div>
          </div>
        </div>
        <div class="stat-badge">Entities</div>
      </div>
      <div class="stat-sub">Global organizations impacted across disclosures.</div>
      <div class="stat-bar"><span style="width: 62%"></span></div>
    </div>

    <div class="glass-card stat-card">
      <div class="stat-top">
        <div class="stat-meta">
          <div class="stat-icon">
            <svg class="svg-icon" viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><path d="M14 2v6h6"></path><path d="M8 13h8"></path><path d="M8 17h6"></path></svg>
          </div>
          <div class="stat-title">
            <small>Research Vectors</small>
            <div class="stat-value">4</div>
          </div>
        </div>
        <div class="stat-badge">Write-ups</div>
      </div>
      <div class="stat-sub">Public technical reports and detailed PoCs.</div>
      <div class="stat-bar"><span style="width: 48%"></span></div>
    </div>

    <div class="glass-card stat-card">
      <div class="stat-top">
        <div class="stat-meta">
          <div class="stat-icon">
            <svg class="svg-icon" viewBox="0 0 24 24"><path d="M20 6L9 17l-5-5"></path></svg>
          </div>
          <div class="stat-title">
            <small>Resolution Rate</small>
            <div class="stat-value">100%</div>
          </div>
        </div>
        <div class="stat-badge">Verified</div>
      </div>
      <div class="stat-sub">Reports accepted and validated by the programs.</div>
      <div class="stat-bar"><span style="width: 100%"></span></div>
    </div>
  </div>

  <div class="contact-panel">
    <div>
      <h3>Contact</h3>
      <p>Want to verify details, collaborate, or ask questions? Reach me here.</p>
    </div>
    <div class="contact-links">
      <a class="contact-link" href="{{ site.contacts.linkedin }}" target="_blank" rel="noopener noreferrer">
        <svg class="svg-icon" viewBox="0 0 24 24"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4V9h4v2"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
        <span class="contact-handle">LinkedIn</span>
      </a>
      <a class="contact-link" href="{{ site.contacts.telegram }}" target="_blank" rel="noopener noreferrer">
        <svg class="svg-icon" viewBox="0 0 24 24"><path d="M22 2L11 13"></path><path d="M22 2L15 22l-4-9-9-4z"></path></svg>
        <span class="contact-handle">Telegram</span>
      </a>
      <a class="contact-link" href="{{ site.contacts.discord }}" target="_blank" rel="noopener noreferrer">
        <svg class="svg-icon" viewBox="0 0 24 24"><path d="M18 4a8 8 0 0 0-3-1l-1 2a9 9 0 0 0-4 0L9 3a8 8 0 0 0-3 1c-2 3-3 7-2 11a9 9 0 0 0 4 2l1-2"></path><path d="M20 15a9 9 0 0 1-4 2l-1-2"></path><path d="M9.5 14a1 1 0 1 0 0-2 1 1 0 0 0 0 2z"></path><path d="M14.5 14a1 1 0 1 0 0-2 1 1 0 0 0 0 2z"></path></svg>
        <span class="contact-handle">Discord</span>
      </a>
    </div>
  </div>

  <footer>
    <p>© 2026 ConnorDev | Research conducted under ethical disclosure guidelines.</p>
  </footer>
</div>