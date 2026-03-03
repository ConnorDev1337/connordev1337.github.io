---
layout: default
---

<div class="container">
<article>
  <div class="lang-switcher">
    <a href="../../../es/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn">Español (ES)</a>
    <a href="./" class="lang-btn active">English (EN)</a>
  </div>

  <div class="bounty-header">
    <h1>NASA JPL: Reflected XSS Cookie Exfiltration</h1>
    <p>Bypassing Web Application Firewalls (WAF) to achieve session takeover on NASA JPL ECCO assets.</p>
  </div>

  <div class="bounty-content">
    <h2>🔍 Initial Discovery</h2>
    <p>The target was NASA's Jet Propulsion Laboratory asset <code>ecco.jpl.nasa.gov</code>. Testing the search functionality revealed reflection of input within a JavaScript context.</p>
    
    <p><code>https://ecco.jpl.nasa.gov/search.htm?search=pwn</code></p>
    
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal.png" alt="Search Reflection">

    <h2>🛡️ WAF Evasion Strategy</h2>
    <p>The WAF initially blocked <code>&lt;script&gt;</code> tags. By breaking the <code>console.log</code> logic and encoding semicolons as <code>%3b</code>, I was able to execute arbitrary JS.</p>
    
    <pre><code>pwn'); // Break the system logic
a=document; // Create var 'a' containing document
alert(a.cookie); // Call alert with cookies
&lt;/script&gt;;// // Close tag and comment rest</code></pre>

    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Bypass%20Semicollon.png" alt="Semicolon Bypass">

    <h2>🚀 Final Exploit Payload</h2>
    <p>The final PoC exfiltrated cookies via <code>fetch()</code> to an external server. The bypass involved using variable references to avoid direct <code>document.cookie</code> strings which were filtered.</p>
    
    <img src="../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration.png" alt="Exfiltration PoC">

    <h2>🎖️ Recognition & Award</h2>
    <p>NASA JPL acknowledged this disclosure with an official Letter of Recognition, confirming the impact and remediation of the vulnerability.</p>

    <div class="pdf-container">
      <iframe src="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" width="100%" height="100%" frameborder="0"></iframe>
    </div>

    <p style="text-align: center; margin-top: 20px;">
      <a href="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" class="lang-btn" target="_blank">📥 Download Official Letter (PDF)</a>
    </p>

    <h2>🤝 Collaboration Detail</h2>
    <p>This research was conducted in a 50/50 collaboration between {{ site.researchers.ivan.name }} and {{ site.researchers.diego.name }}.</p>
  </div>
</article>
</div>
