# Detector de Sitios Hackeados y Spam SEO

[![Detector Spam SEO](https://img.shields.io/badge/TAVARDT-Antispam%20SEO-5b3cf5.svg)](https://tools.tavardt.com/es/detector-seo-spam-sitio-hackeado/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

El **TAVARDT Detector de Sitios Hackeados y Spam SEO** proporciona inspección forense en aplicaciones web para detectar cloaking hacia buscadores, inyecciones ocultas de enlaces de casino o farmacia, iframes no autorizados y scripts ofuscados en España.

***

## Arquitectura de Detección y Matriz Forense de Vectores

El cloaking condicional y el contenido inyectado manipulan a los rastreadores de búsqueda mientras se mantienen ocultos a los usuarios legítimos. Esta intrusión provoca penalizaciones manuales en Google, pérdida total de indexación y daño reputacional corporativo.

Esta herramienta de diagnóstico audita la divergencia de respuestas entre User-Agents, analiza contenedores CSS ocultos y aísla vectores de seguimiento no autorizados sin librerías externas.

El motor de análisis evalúa la integridad del DOM, la variación de volumen en las respuestas y firmas de scripts sospechosos a partir de 100 puntos iniciales:

<table>
  <thead>
    <tr>
      <th>Vector de Compromiso</th><th>Lógica de Detección</th><th>Deducción</th><th>Impacto en la Integridad</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Cloaking Diferencial</strong></td><td>Compara User-Agent de navegador estándar con Googlebot. Detectado si la tasa de divergencia de bytes > 0,40 y el delta absoluto > 1000 bytes.</td><td>-30 pts</td><td>Riesgo crítico de penalización algorítmica y pérdida total de posicionamiento orgánico.</td>
    </tr>
    <tr>
      <td><strong>Inyección de Spam Oculto</strong></td><td>Inspecciona contenedores con <code>display:none</code>, <code>opacity:0</code> o márgenes negativos con términos de farmacia y apuestas.</td><td>-35 pts</td><td>Confirmación inequívoca de intrusión en base de datos o plantillas manipuladas.</td>
    </tr>
    <tr>
      <td><strong>iframes Ocultos Sospechosos</strong></td><td>Identifica iframes con dimensiones cero o visibilidad oculta fuera de la lista blanca de servicios de seguridad legítimos.</td><td>-20 pts</td><td>Riesgo de descarga silenciosa de malware, redirección indebida y robo de credenciales.</td>
    </tr>
    <tr>
      <td><strong>Código Ofuscado</strong></td><td>Detecta patrones de ejecución dinámica repetidos como <code>eval()</code>, <code>unescape()</code> y <code>base64_decode</code> (>2 apariciones).</td><td>-15 pts</td><td>Comportamiento asociado a droppers y scripts de persistencia maliciosa en el servidor.</td>
    </tr>
    <tr>
      <td><strong>Fallo de Conexión</strong></td><td>Tiempo de espera agotado o error en el protocolo TLS/HTTP durante la negociación.</td><td>-20 pts</td><td>Imposibilidad de acceder al host impidiendo la verificación forense completa.</td>
    </tr>
  </tbody>
</table>

### Escala de Integridad y Niveles de Estado

La herramienta determina un índice consolidado de salud e integridad para el sitio web:

* <strong>85 a 100 (CLEAN)</strong>: Sin indicios de compromiso detectados; comportamiento coherente y consistente entre User-Agents.
* <strong>60 a 84 (SUSPICIOUS)</strong>: Se detectan anomalías en hojas de estilo, iframes sospechosos o diferencias parciales que requieren auditoría.
* <strong>0 a 59 (COMPROMISED)</strong>: Presencia confirmada de cloaking, inyecciones de spam o scripts ofuscados que exigen saneamiento urgente.

***

## Instalación y Ejecución

Construido íntegramente con la librería estándar de Python 3.8+ sin dependencias externas.

```bash
git clone https://github.com/TAVARDT-Espana/hacked-site-seo-spam-detector-es.git
cd hacked-site-seo-spam-detector-es
```

### Ejecución Estándar CLI

```bash
python seo_spam_detector.py ejemplo.es
```

### Salida Estructurada para Pipelines en JSON

```bash
python seo_spam_detector.py ejemplo.es -j
```

### Configuración de Timeout Personalizado

```bash
python seo_spam_detector.py ejemplo.es -j -t 15
```

***

## Alineación ENS e INCIBE para la Resiliencia Web

Este analizador se alinea con las directrices del **Esquema Nacional de Seguridad (ENS)** y las recomendaciones de ciberseguridad del **INCIBE**. Detectar a tiempo modificaciones no autorizadas en el código web garantiza la continuidad del servicio.

Se recomienda incorporar comprobaciones automatizadas de cloaking e integridad del DOM en las auditorías periódicas de seguridad de activos digitales corporativos.

***

## Preguntas Frecuentes Técnicas

### ¿Cómo opera el algoritmo de detección de cloaking diferencial?

La utilidad emite dos peticiones HTTP consecutivas al destino, primero con el User-Agent de un navegador moderno y luego simulando el rastreador Googlebot. Si la discrepancia relativa de tamaño excede 0,40 y la variación absoluta supera los 1000 bytes, se reporta cloaking y se deducen 30 puntos.

### ¿Qué factores clasifican un iframe oculto como sospechoso?

Cualquier iframe con dimensiones nulas o reglas CSS de ocultación (<code>display:none</code>) que no provenga de proveedores de seguridad acreditados (como Cloudflare, Google reCAPTCHA, Google Tag Manager o hCaptcha) es catalogado como sospechoso con deducción de 20 puntos.

### ¿Cuáles términos de spam se rastrean en bloques CSS ocultos?

El analizador revisa contenedores ocultos mediante <code>display:none</code>, <code>visibility:hidden</code>, <code>opacity:0</code> o márgenes negativos en busca de palabras como viagra, cialis, casino, payday loan, replica watch, poker, roulette, baccarat, vape y ketamine, aplicando una sanción de 35 puntos.

### ¿Por qué se penalizan funciones como eval y unescape?

Las aplicaciones web en producción no deben ejecutar llamadas a <code>eval()</code> o <code>unescape()</code> en su código frontal. Más de dos coincidencias sugieren la ejecución de payloads dinámicos inyectados por atacantes, generando una penalización de 15 puntos.

### ¿Cómo se consume la salida del validador en sistemas automatizados?

Al invocar el script con el parámetro <code>-j</code>, la herramienta emite un documento JSON estructurado por la salida estándar, facilitando su consumo en plataformas SIEM, alertas automáticas o canalizaciones de despliegue continuo.

***

## Desinfección y Respuesta ante Incidentes

Para asistencia técnica en saneamiento de sitios comprometidos y cumplimiento de Directrices ENS e Integridad Web, consulte a los especialistas de **TAVARDT**.

***

## Enlaces Útiles

* **Portal Corporativo:** https://ag.tavardt.com/es/
* **Suite de Diagnósticos:** https://tools.tavardt.com/es/
* **Contacto Técnico:** https://ag.tavardt.com/es/contacto/

***

### Licencia
Distribuido bajo la Licencia MIT.

***

© 2026 TAVARDT. Todos los derechos reservados.
