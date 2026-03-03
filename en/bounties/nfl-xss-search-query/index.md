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
    <p>The NFL's main search functionality was found to be vulnerable to Reflected Cross-Site Scripting (XSS). The vulnerability resided in how the application handled and reflected the search query parameter back into the page without proper output encoding.</p>

    <h2>⚙️ Technical Details</h2>
    <ul>
      <li><strong>Affected URL:</strong> <code>https://www.nfl.com/search/?query=[XSS_PAYLOAD]</code></li>
      <li><strong>Affected Parameter:</strong> <code>query</code></li>
      <li><strong>Vector:</strong> Client-side injection of malicious scripts via URL.</li>
    </ul>

    <h2>🛠️ Steps to Reproduce</h2>
    <p>By injecting a standard script tag into the <code>query</code> parameter, we confirmed that the input was reflected directly into the Document Object Model (DOM):</p>
    
    <pre><code>"><script>alert(document.domain)</script></code></pre>
    
    <img src="../../../assets/images/nfl-xss-search-query/PayloadExecution.png" alt="XSS Payload Execution">

    <p>The application failed to sanitize the input, allowing the script to execute in the context of the victim's browser. This could be leveraged to steal sensitive session information or perform actions on behalf of the user.</p>

    <h2>⚠️ Security Impact</h2>
    <p>An attacker could craft a malicious link and send it to an authenticated user. Once clicked, the attacker could:</p>
    <ul>
      <li>Exfiltrate session cookies and bypass authentication.</li>
      <li>Perform unauthorized actions (CSRF-like impact via XSS).</li>
      <li>Deface the website content for specific users.</li>
    </ul>

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
        <strong>P3 (Medium)</strong>
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
