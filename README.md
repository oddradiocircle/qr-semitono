# QR en semitono

Página web que crea un código QR con una imagen dentro, dibujada con una trama de puntos. El código se sigue pudiendo escanear con el teléfono.

## Cómo usarla

1. Abre `index.html` en el navegador. No necesita instalación ni servidor.
2. Elige qué hace el código: enlace, texto, WhatsApp, wifi, contacto, correo, llamada, SMS, ubicación o evento.
3. Toma una foto o carga una imagen.
4. Ajusta el resultado: zoom, posición, brillo, contraste, tonos medios, protección contra daños, detalle y prioridad a la imagen.
5. Descarga el código en PNG con el botón "Descargar imagen (PNG)".

La página prueba cada código con un lector de QR, nítido y un poco borroso. Esa prueba no reemplaza a un teléfono real: escanea el código antes de imprimirlo.

La imagen no sale del equipo. Todo pasa en el navegador.

## Cómo funciona

Un lector de QR mira solo el centro de cada cuadro del código. La página divide cada cuadro en 9 puntos:

- El punto del centro guarda el dato del código.
- Los 8 puntos de alrededor dibujan la imagen con difusión de error (Floyd–Steinberg).
- Los cuadros de posición, alineación, tiempo y formato quedan sólidos, porque el lector los usa para ubicar el código.

Con "Prioridad a la imagen" mayor que 0 %, algunos centros pasan a la imagen. La corrección de errores del QR repara esos datos. Con la imagen de ejemplo y protección máxima, el código se leyó hasta con 6 % (prueba en navegador automático, 2026-11-07).

## Dependencias

Se cargan desde jsDelivr, así que la página necesita internet:

| Librería | Versión | Uso | Licencia |
|---|---|---|---|
| [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) | 1.4.4 | Crear el QR | MIT |
| [jsQR](https://github.com/cozmo/jsQR) | 1.4.0 | Probar que el código se lee | Apache 2.0 |

Las fuentes vienen de Google Fonts. Si no cargan, la página usa fuentes del sistema.

## Créditos

- Técnica: H.-K. Chu, C.-S. Chang, R.-R. Lee y N. J. Mitra, "Halftone QR Codes", ACM SIGGRAPH Asia 2013. [doi:10.1145/2508363.2508408](https://doi.org/10.1145/2508363.2508408).
- Fuente de la idea: [Hackaday, 2026-11-06](https://hackaday.com/2026/11/06/using-halftone-images-in-qr-codes-without-upsetting-scanners/).
- Implementación de referencia en JavaScript: [Jorge C. Lucero](https://lucerojc.github.io/halftone-qr/). Este código es una versión propia, no una copia.

## Pendientes

- Probar con teléfonos reales los tipos Wi-Fi, Contacto y Evento en iPhone y Android.

## Licencia

GPL-3.0. Ver [LICENSE](LICENSE). Las librerías externas conservan sus propias licencias (MIT y Apache 2.0), compatibles con GPL-3.0.
