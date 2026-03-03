---
layout: default
submission_id: "e8471382-6e56-4e74-b4e2-ff20279371ff"
official_link: "https://bugcrowd.com/submissions/e8471382-6e56-4e74-b4e2-ff20279371ff"
---

<div class="container">
  <div class="lang-switcher">
    <a href="./" class="lang-btn active">
      <span class="label-full">English (EN)</span>
      <span class="label-short">EN</span>
    </a>
    <a href="/es/bounties/nfl-xss-search-query/" class="lang-btn">
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
    <h1>Reflected XSS in Search Functionality</h1>
    <p style="color: var(--p3); font-weight: 600;">
      National Football League (NFL)
    </p>
  </div>

  <div class="bounty-content">

    <h2>📌 Context</h2>
    <p>
      During the security assessment of the public subdomain 
      <code>hbcutournament.nfl.com</code>, 
      the search functionality available to unauthenticated users was analyzed.
    </p>

    <p>
      The analysis revealed that the search parameter in the URL reflected 
      user-supplied input directly within the rendered HTML response 
      without proper contextual encoding.
    </p>

    <h2>🔎 Technical Identification</h2>
    <p>
      The vulnerability was located in the following parameter:
    </p>

    <pre><code>https://hbcutournament.nfl.com/{resources,blogs}?q=</code></pre>

    <p>
      The value of the <code>q</code> parameter was embedded in the page output 
      without sufficient sanitization, allowing user-controlled content 
      to be interpreted as part of the DOM.
    </p>

    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - HTML Injection.png" alt="Reflected Input">

    <p>
      Controlled testing confirmed that the browser interpreted certain 
      injected HTML structures supplied through the search parameter.
    </p>

    <img src="/assets/images/nfl-xss-search-query/NFL - XSS Search Query Parameter - Alert 1.png" alt="Client-Side Execution">

    <h2>⚙️ Behavior Analysis</h2>
    <p>
      The rendering flow did not implement proper context-aware encoding 
      (HTML, attribute, or JavaScript context).
    </p>

    <p>
      As a result, specially crafted input could be interpreted by the browser 
      as executable content rather than plain text.
    </p>

    <p>
      No authentication was required to trigger the vulnerable behavior.
    </p>

    <h2>🔐 Potential Impact</h2>
    <p>
      Prior to remediation, this vulnerability could have enabled:
    </p>

    <ul>
      <li>Execution of arbitrary JavaScript in a victim’s browser.</li>
      <li>DOM manipulation within a trusted domain context.</li>
      <li>Brand-trust abuse for phishing scenarios.</li>
      <li>Access to data available within the browser context, depending on security configuration.</li>
    </ul>

    <p>
      No evidence of active exploitation was identified.
    </p>

    <h2>🛠️ Remediation Process</h2>
    <p>
      The issue was responsibly reported through the official bug bounty 
      program managed on Bugcrowd.
    </p>

    <p>
      After validation, the technical team implemented:
    </p>

    <ul>
      <li>Proper contextual encoding of reflected parameters.</li>
      <li>Strengthened server-side input validation.</li>
      <li>Improvements to dynamic rendering logic in the search component.</li>
    </ul>

    <p>
      Following remediation, the vulnerability was no longer reproducible.
    </p>

    <h2>📚 Technical Considerations</h2>
    <p>
      This case reinforces several important security principles:
    </p>

    <ul>
      <li>Search parameters are common injection vectors.</li>
      <li>Blacklist-based filtering is insufficient against XSS.</li>
      <li>Context-aware output encoding is essential.</li>
      <li>Public-facing functionality must be treated as an active attack surface.</li>
    </ul>

    <h2>🤝 Collaboration Details</h2>
    <p>
      This research was conducted in collaboration with the following researchers:
    </p>

    <div class="collab-grid">
      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
          </svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.ivan.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.ivan.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.ivan.linkedin }}" target="_blank" rel="noopener noreferrer">
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>

      <div class="collab-card">
        <div class="collab-avatar">
          <svg class="svg-icon" viewBox="0 0 24 24">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
          </svg>
        </div>
        <div class="collab-body">
          <div class="collab-name">{{ site.researchers.diego.name }}</div>
          <div class="collab-links">
            <a class="social-pill" href="{{ site.researchers.diego.bugcrowd }}" target="_blank" rel="noopener noreferrer">
              <span>Bugcrowd</span>
            </a>
            <a class="social-pill" href="{{ site.researchers.diego.linkedin }}" target="_blank" rel="noopener noreferrer">
              <span>LinkedIn</span>
            </a>
          </div>
        </div>
      </div>
    </div>

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