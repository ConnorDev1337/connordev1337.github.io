---
layout: default
title: ConnorDev | Bug Bounty Portfolio
---

<div class="container">
  <div class="lang-switcher">
    <a href="../en/" class="lang-btn"><span class="label-full">English (EN)</span><span class="label-short">EN</span></a>
    <a href="./" class="lang-btn active"><span class="label-full">Español (ES)</span><span class="label-short">ES</span></a>
  </div>

  <header>
    <h1>{{ site.title }}</h1>
    <p style="color: var(--text-secondary); font-size: 1.1rem; max-width: 800px;">
      {{ site.description_es }}
    </p>
  </header>

  <h2 class="section-title">Cronología</h2>
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
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/dc8e/976c/23d576ba/cbf9d3c4e7dc5a974e27565bad46ae76_national_football_league_logo.jpeg" alt="NFL">NFL</td>
          <td class="col-vuln">XSS Reflejado en Parámetro de Query de Búsqueda en URL</td>
          <td class="col-severity"><span class="badge badge-p3">Media</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-success" viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nfl-xss-search-query/" class="icon-link" title="Ver Reporte">
              <svg class="svg-icon icon-view" viewBox="0 0 24 24"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path><circle cx="12" cy="12" r="3"></circle></svg>
            </a>
          </td>
        </tr>
        <tr>
          <td class="col-date">2025-09-25</td>
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/3f7e/826a/d0f78ced/a58f55915330f1e5d19dc75e462129d6_university_of_canterbury_logo.jpeg" alt="UC">University of Canterbury</td>
          <td class="col-vuln"><code>[CONFIDENCIAL / INVESTIGACIÓN]</code></td>
          <td class="col-severity"><span class="badge badge-p2">Alta</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-pending" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>
          </td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-18</td>
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/df14/f8f6/e5217321/d6766f0d7e4864eca4d059e018e16bb7_U.S._International_Trade_Commission.png" alt="USITC">US ITC</td>
          <td class="col-vuln"><code>[CONFIDENCIAL / INVESTIGACIÓN]</code></td>
          <td class="col-severity"><span class="badge badge-p3">Media</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-pending" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>
          </td>
          <td class="col-report">-</td>
        </tr>
        <tr>
          <td class="col-date">2025-09-16</td>
          <td class="col-company"><img class="company-logo" src="https://logos.bugcrowdusercontent.com/logos/e34e/ced8/462922dd/fe2d0bf28f7b56095470ca07d0421e95_1615217311496.jpeg" alt="NASA">NASA JPL ECCO</td>
          <td class="col-vuln">XSS en Formulario de Búsqueda con Exfiltración de Cookies (WAF Bypass)</td>
          <td class="col-severity"><span class="badge badge-p3">Media</span></td>
          <td class="col-status">
            <svg class="svg-icon icon-success" viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg>
          </td>
          <td class="col-report">
            <a href="./bounties/nasa-jpl-ecco-xss-waf-bypass/" class="icon-link" title="Ver Reporte">
              <svg class="svg-icon icon-view" viewBox="0 0 24 24"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path><circle cx="12" cy="12" r="3"></circle></svg>
            </a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <h2 class="section-title section-mt">Muro de la Fama</h2>
  <div class="wof-grid">
    <a href="https://bugcrowd.com/engagements/nfl-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="NFL Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/dc8e/976c/23d576ba/cbf9d3c4e7dc5a974e27565bad46ae76_national_football_league_logo.jpeg" alt="NFL">
    </a>
    <a href="https://bugcrowd.com/engagements/university-canterbury-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="University of Canterbury Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/3f7e/826a/d0f78ced/a58f55915330f1e5d19dc75e462129d6_university_of_canterbury_logo.jpeg" alt="University of Canterbury">
    </a>
    <a href="https://bugcrowd.com/engagements/usitc-vdp/hall_of_fames" target="_blank" class="wof-card" title="US International Trade Commission Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/df14/f8f6/e5217321/d6766f0d7e4864eca4d059e018e16bb7_U.S._International_Trade_Commission.png" alt="US ITC">
    </a>
    <a href="https://bugcrowd.com/engagements/nasa-vdp/hall_of_fames" target="_blank" class="wof-card" title="NASA Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/e34e/ced8/462922dd/fe2d0bf28f7b56095470ca07d0421e95_1615217311496.jpeg" alt="NASA">
    </a>
    <a href="https://bugcrowd.com/engagements/gamingcorps-vdp-pro/hall_of_fames" target="_blank" class="wof-card" title="Gaming Corps Hall of Fame">
      <img src="https://logos.bugcrowdusercontent.com/logos/3e27/2dae/d8b0a7b5/a44c8386bc03fa99c885f02a925f0d81_gaming_corps_logo.jpeg" alt="Gaming Corps">
    </a>
  </div>

  <div class="contact-panel">
    <div>
      <h3>Contacto</h3>
      <p>¿Quieres verificar detalles, colaborar o hacer preguntas? Escríbeme por aquí.</p>
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
        <svg class="svg-icon svg-fill" viewBox="0 -28.5 256 256" version="1.1" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="xMidYMid" fill="currentColor"><path d="M216.856339,16.5966031 C200.285002,8.84328665 182.566144,3.2084988 164.041564,0 C161.766523,4.11318106 159.108624,9.64549908 157.276099,14.0464379 C137.583995,11.0849896 118.072967,11.0849896 98.7430163,14.0464379 C96.9108417,9.64549908 94.1925838,4.11318106 91.8971895,0 C73.3526068,3.2084988 55.6133949,8.86399117 39.0420583,16.6376612 C5.61752293,67.146514 -3.4433191,116.400813 1.08711069,164.955721 C23.2560196,181.510915 44.7403634,191.567697 65.8621325,198.148576 C71.0772151,190.971126 75.7283628,183.341335 79.7352139,175.300261 C72.104019,172.400575 64.7949724,168.822202 57.8887866,164.667963 C59.7209612,163.310589 61.5131304,161.891452 63.2445898,160.431257 C105.36741,180.133187 151.134928,180.133187 192.754523,160.431257 C194.506336,161.891452 196.298154,163.310589 198.110326,164.667963 C191.183787,168.842556 183.854737,172.420929 176.223542,175.320965 C180.230393,183.341335 184.861538,190.991831 190.096624,198.16893 C211.238746,191.588051 232.743023,181.531619 254.911949,164.955721 C260.227747,108.668201 245.831087,59.8662432 216.856339,16.5966031 Z M85.4738752,135.09489 C72.8290281,135.09489 62.4592217,123.290155 62.4592217,108.914901 C62.4592217,94.5396472 72.607595,82.7145587 85.4738752,82.7145587 C98.3405064,82.7145587 108.709962,94.5189427 108.488529,108.914901 C108.508531,123.290155 98.3405064,135.09489 85.4738752,135.09489 Z M170.525237,135.09489 C157.88039,135.09489 147.510584,123.290155 147.510584,108.914901 C147.510584,94.5396472 157.658606,82.7145587 170.525237,82.7145587 C183.391518,82.7145587 193.761324,94.5189427 193.539891,108.914901 C193.539891,123.290155 183.391518,135.09489 170.525237,135.09489 Z"/></svg>
        <span class="contact-handle">Discord</span>
      </a>
      <a class="contact-link" href="{{ site.contacts.hackthebox }}" target="_blank" rel="noopener noreferrer">
        <svg class="svg-icon svg-fill" viewBox="0 0 24 24" role="img" xmlns="http://www.w3.org/2000/svg" fill="currentColor"><path d="M11.996 0a1.119 1.119 0 0 0-.057.003.9.9 0 0 0-.236.05.907.907 0 0 0-.165.079L1.936 5.675a.889.889 0 0 0-.445.77v11.111a.889.889 0 0 0 .47.784l9.598 5.541.054.029v.002a.857.857 0 0 0 .083.035l.012.004c.028.01.056.018.085.024.01.001.011.003.016.004a.93.93 0 0 0 .296.015.683.683 0 0 0 .086-.015c.01 0 .011-.002.016-.004a.94.94 0 0 0 .085-.024l.012-.004a.882.882 0 0 0 .083-.035v-.002a1.086 1.086 0 0 0 .054-.029l9.599-5.541a.889.889 0 0 0 .469-.784V6.48l-.001-.026v-.008a.889.889 0 0 0-.312-.676l-.029-.024c0-.002-.01-.005-.01-.007a.899.899 0 0 0-.107-.07L12.453.127A.887.887 0 0 0 11.99 0zm.01 2.253c.072 0 .144.019.209.056l6.537 3.774a.418.418 0 0 1 0 .724l-6.537 3.774a.418.418 0 0 1-.418 0L5.26 6.807a.418.418 0 0 1 0-.724l6.537-3.774a.42.42 0 0 1 .209-.056zm-8.08 6.458a.414.414 0 0 1 .215.057l6.524 3.766a.417.417 0 0 1 .208.361v7.533a.417.417 0 0 1-.626.361l-6.523-3.766a.417.417 0 0 1-.209-.362V9.13c0-.241.196-.414.41-.418zm16.16 0c.215.004.41.177.41.418v7.532a.42.42 0 0 1-.208.362l-6.524 3.766a.417.417 0 0 1-.626-.361v-7.533c0-.149.08-.286.209-.36l6.523-3.767a.415.415 0 0 1 .216-.057z"/></svg>
        <span class="contact-handle">HackTheBox</span>
      </a>
    </div>
  </div>

  <footer>
    <p>© 2026 ConnorDev | Investigación realizada bajo pautas de divulgación ética.</p>
  </footer>
</div>
