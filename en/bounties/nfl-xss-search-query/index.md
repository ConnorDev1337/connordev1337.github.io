---
layout: default
---

<div class="container">
<article>
  <a href="../../../en/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    Cyber Research Portfolio
  </a>
  <div class="lang-switcher">
    <a href="../../../es/bounties/nfl-xss-search-query/" class="lang-btn">Español (ES)</a>
    <a href="./" class="lang-btn active">English (EN)</a>
  </div>

  <div class="bounty-header">
    <h1>NFL: Reflected XSS Search Query</h1>
    <p>Discovering and exploiting unescaped search parameters on the National Football League's digital assets.</p>
  </div>

  <div class="bounty-content">
    <h2>🔍 Vulnerability Research</h2>
    <p>Testing for reflection on <code>hbcutournament.nfl.com</code> revealed that input via the <code>?q=</code> parameter was directly rendered into the DOM without sanitization.</p>
    <p><code>https://hbcutournament.nfl.com/resources?q=</code></p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20HTML%20Injection.png" alt="HTML Injection Confirm">

    <h2>🚨 XSS Vector Execution</h2>
    <p>Arbitrary JS execution was achieved using <code>img</code> tags without a source, triggering the <code>onerror</code> event handler.</p>
    <pre><code>&lt;img/src/onerror=alert(8)&gt;</code></pre>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Alert%201.png" alt="XSS Trigger">

    <h2>🚀 Critical Data Exfiltration</h2>
    <p>By chaining the XSS with <code>fetch()</code>, I successfully exfiltrated <code>document.cookie</code> data to an attacker-controlled server, demonstrating high impact session takeover potential.</p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration.png" alt="Cookie Hijacking">
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration%20Result.png" alt="Server Logs Received">

    <h2>🤝 Collaboration Detail</h2>
    <p>This research was conducted in a 50/50 collaboration between {{ site.researchers.ivan.name }} and {{ site.researchers.diego.name }}.</p>
  </div>
</article>
</div>

<div class="zoom-overlay" id="zoomOverlay" onclick="this.classList.remove('active')">
  <img id="zoomImg" src="" alt="Zoom">
</div>
<script>
document.querySelectorAll('.bounty-content img').forEach(img => {
  img.addEventListener('click', () => {
    document.getElementById('zoomImg').src = img.src;
    document.getElementById('zoomOverlay').classList.add('active');
  });
});
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') document.getElementById('zoomOverlay').classList.remove('active');
});
</script>
