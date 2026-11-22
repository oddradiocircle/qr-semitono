# QR en semitono

Página web que crea un código QR con una imagen dentro, dibujada con una trama de puntos. También crea códigos QR animados en GIF a partir de un video. El código se sigue pudiendo escanear con el teléfono.

**Úsala en línea:** <https://oddradiocircle.github.io/qr-semitono/>

## Cómo usarla

1. Abre la página en línea, o abre `index.html` en el navegador. No necesita instalación ni servidor.
2. Elige qué hace el código: enlace, texto, WhatsApp, wifi, contacto, correo, llamada, SMS, ubicación o evento.
3. Toma una foto o carga una imagen. Para un código animado, elige **Video** y graba o carga un video.
4. Ajusta el resultado: zoom, posición, brillo, contraste, tonos medios, protección contra daños, detalle y prioridad a la imagen.
5. Descarga el código en PNG. En modo video, toca "Crear GIF" y luego "Descargar GIF".

La página prueba cada código con un lector de QR, nítido y un poco borroso. Esa prueba no reemplaza a un teléfono real: escanea el código antes de imprimirlo.

La imagen no sale del equipo. Todo pasa en el navegador.

## Cómo funciona

Un lector de QR mira solo el centro de cada cuadro del código. La página divide cada cuadro en puntos:

- Los puntos del centro guardan el dato del código.
- Los puntos de alrededor dibujan la imagen con difusión de error (Floyd–Steinberg).
- Los cuadros de posición, alineación, tiempo y formato quedan sólidos, porque el lector los usa para ubicar el código.

"Facilidad de lectura" define cuántos puntos tiene cada cuadro y cuántos guardan el dato:

| Opción | Puntos por cuadro | Puntos del centro | Parte del cuadro que guarda el dato |
|---|---|---|---|
| Más imagen (inicial) | 3 × 3 | 1 | 11 % |
| Equilibrada | 4 × 4 | 2 × 2 | 25 % |
| Más lectura | 5 × 5 | 3 × 3 | 36 % |

Con un centro más grande, la cámara lee más fácil, pero la imagen pierde detalle y se ven cuadros más grandes. Conviene empezar con "Más imagen" y subir solo si la cámara tiene problemas.

Con "Prioridad a la imagen" mayor que 0 %, algunos centros pasan a la imagen. La corrección de errores del QR repara esos datos. Con la imagen de ejemplo y protección máxima, el código se leyó hasta con 6 % (prueba en navegador automático, 2026-11-07).

## Modo video

Cada cuadro del GIF es un código QR con los mismos datos. Los puntos centrales no cambian; solo se mueven los de alrededor. Así el teléfono puede leer cualquier cuadro.

- **Clip:** inicio, duración (máximo 6 segundos) y cuadros por segundo (8, 10, 12 o 15).
- **Forma de repetir:**
  - "Fundido" (inicial) mezcla el final con el inicio durante el primer 25 % del GIF, para que la vuelta no se note. Usa unos segundos extra del video después del clip; si no los hay, el GIF queda un poco más corto.
  - "Ida y vuelta" reproduce el clip hacia adelante y luego hacia atrás. Dura y pesa casi el doble.
  - "Corte" vuelve al inicio de golpe.
- **Vista previa:** la página prepara los cuadros del clip una vez. La vista previa y el GIF usan los mismos cuadros, así que lo que ves es lo que descargas.
- **Tipo de trama:** "Fija" usa una trama ordenada (Bayer 8×8) para que los puntos no tiemblen entre cuadros. "Difusa" usa Floyd–Steinberg: más detalle, pero los puntos tiemblan.
- **Prioridad a la imagen:** los cuadros cedidos se eligen con el primer cuadro del clip y no cambian durante el GIF.
- **Revisión:** al crear el GIF, la página prueba cada cuadro con el lector, nítido y un poco borroso, y dice cuántos se leen.
- **Formato:** GIF de 2 colores. El codificador está escrito en la página; no usa librerías externas.

Prueba en navegador automático (2026-11-22, facilidad de lectura "Equilibrada", 3 segundos, 10 cuadros por segundo): con el video de ejemplo y con 2 videos MP4, se leyeron todos los cuadros con las 3 formas de repetir. Con un video de movimiento continuo, el salto al volver al inicio fue de 26,8 con "Corte", 2,3 con "Fundido" y 1,1 con "Ida y vuelta". El cambio normal entre cuadros seguidos fue de 1,8. La escala es la diferencia media de gris entre cuadros, de 0 a 255. Los GIF pesaron entre 371 y 471 KB, o entre 718 y 910 KB con "Ida y vuelta" (520 × 520 píxeles).

Riesgo conocido: Instagram, WhatsApp y otras apps vuelven a comprimir los videos y los GIF. Esa compresión puede borrar los puntos finos y dañar la lectura. Pruébalo después de subirlo.

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
- Escanear un GIF desde una pantalla con iPhone y Android.
- Probar si el GIF se sigue leyendo después de subirlo a WhatsApp e Instagram.

## Licencia

GPL-3.0. Ver [LICENSE](LICENSE). Las librerías externas conservan sus propias licencias (MIT y Apache 2.0), compatibles con GPL-3.0.
