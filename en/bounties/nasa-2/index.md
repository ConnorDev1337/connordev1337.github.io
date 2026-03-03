---
layout: default
submission_id: "4b01725e-288b-4db5-a38c-1a37f827d215"
official_link: "https://bugcrowd.com/submissions/4b01725e-288b-4db5-a38c-1a37f827d215"
---

<div class="container">
  <div class="lang-switcher">
    <a href="./" class="lang-btn active">
      <span class="label-full">English (EN)</span>
      <span class="label-short">EN</span>
    </a>
    <a href="/es/bounties/nasa-jpl-ecco-xss-waf-bypass/" class="lang-btn">
      <span class="label-full">Español (ES)</span>
      <span class="label-short">ES</span>
    </a>
  </div>

<article>
  <a href="/en/" class="back-btn">
    <svg viewBox="0 0 24 24">
      <polyline points="15 18 9 12 15 6"></polyline>
    </svg>
    {{ site.title }}
  </a>

  <div class="bounty-header">
    <h1>Reflected XSS in Search Functionality with Input Filter Bypass</h1>
    <p style="color: var(--p3); font-weight: 600;">
      National Aeronautics and Space Administration – Jet Propulsion Laboratory (NASA JPL)
    </p>
  </div>

  <div class="bounty-content">

    <h2>📌 Context</h2>
    <p>
      During the security assessment of a publicly accessible NASA JPL subdomain,
      the search functionality was analyzed as part of routine input handling review.
    </p>

    <p>
      The assessment revealed that user-controlled input supplied through the search parameter
      was reflected within a client-side JavaScript context without proper contextual encoding.
    </p>

    <h2>🔎 Technical Identification</h2>
    <p>
      The vulnerable endpoint processed user input from the query string and embedded it
      directly into dynamically generated JavaScript.
    </p>

    <p>
      Because the application relied on pattern-based filtering rather than strict contextual
      output encoding, specially crafted input could alter the intended execution context.
    </p>

    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal.png" alt="Search Reflection">

    <p>
      Inspection of the page source confirmed that user input was inserted into
      a JavaScript function call without adequate encoding safeguards.
    </p>

    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Normal Result.png" alt="JavaScript Context">

    <h2>🛡️ Filter and WAF Analysis</h2>
    <p>
      The application implemented filtering mechanisms and Web Application Firewall (WAF)
      protections designed to block common XSS patterns.
    </p>

    <p>
      However, the defensive controls were primarily keyword-based and did not fully
      account for contextual encoding edge cases.
    </p>

    <img src="/assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS Search - ecco.jpl.nasa.gov - Search Script Tag.png" alt="WAF Blocking">

    <p>
      Under specific crafted conditions, it was possible to bypass the filtering logic
      and achieve JavaScript execution within the browser.
    </p>

    <h2>🔐 Potential Impact</h2>
    <p>
      Prior to remediation, this vulnerability could have enabled:
    </p>

    <ul>
      <li>Execution of arbitrary JavaScript in a victim’s browser.</li>
      <li>DOM manipulation within a trusted domain context.</li>
      <li>Session-related data exposure depending on cookie configuration.</li>
      <li>Client-side phishing or content spoofing scenarios.</li>
    </ul>

    <p>
      No evidence of real-world exploitation was observed.
    </p>

    <h2>🛠️ Remediation</h2>
    <p>
      The issue was responsibly disclosed through the official vulnerability
      disclosure program.
    </p>

    <p>
      After validation, the remediation included:
    </p>

    <ul>
      <li>Proper context-aware output encoding.</li>
      <li>Strengthened server-side validation controls.</li>
      <li>Adjustments to WAF rule handling.</li>
      <li>Improved rendering logic within the search component.</li>
    </ul>

    <p>
      Following remediation, the vulnerable behavior was no longer reproducible.
    </p>

    <h2>📜 Official Acknowledgment</h2>
    <div class="pdf-container">
      <iframe src="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf#toolbar=0&navpanes=0&scrollbar=0"
              width="100%" height="100%" style="border: none;"></iframe>
    </div>

    <div style="text-align: center; margin-top: 1rem;">
      <a href="/assets/others/nasa-jpl-ecco-xss-waf-bypass/acknowledgment.pdf"
         class="lang-btn" target="_blank">
        📥 Download Official Acknowledgment (PDF)
      </a>
    </div>

    <h2>📚 Technical Takeaways</h2>
    <ul>
      <li>Context-aware output encoding is essential for XSS prevention.</li>
      <li>Keyword-based filtering alone is insufficient.</li>
      <li>WAF protections should complement, not replace, secure coding practices.</li>
      <li>Public search functionality represents an active attack surface.</li>
    </ul>

    <div class="meta-grid">
      <div class="meta-card">
        <div class="meta-left">
          <div class="meta-title">
            <strong>Status</strong>
          </div>
        </div>
        <div class="meta-badge success">Resolved</div>
      </div>

      <div class="meta-card">
        <div class="meta-left">
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
              <a href="{{ page.official_link }}" target="_blank" rel="noopener noreferrer">
                {{ page.official_link }}
              </a>
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