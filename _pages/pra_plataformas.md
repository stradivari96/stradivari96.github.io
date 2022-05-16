---
layout: single
permalink: /pra-plataformas/
title: "Plataformas de publicación y distribución: Práctica"
author_profile: true
toc: true
disallow: true
---

Esta página no se encuentra indexada en Google
{: .notice--warning}

## Ejercicio 1: Clip de vídeo incrustado en HTML 5

### Tarea 1.1: Codificar clip de 20 segundos para publicación en web.

Consultando las siguientes referencias:

- [Mozilla](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs)
- [Wikipedia](https://en.wikipedia.org/wiki/HTML5_video#Browser_support)

Consideramos que a día de hoy (Mayo 2022), la mejor opción sería codificar el vídeo en los siguientes códecs y contenedores:

- **AV1/WebM**: el formato más moderno y eficiente hasta la fecha, el mayor problema sería su incompatibilidad con Safari y que
  aún no se ha optimizado el rendimiento de sus codificadores y decodificadores.
  > The primary drawback to AV1 at this time is that it is very new, and support is still in the process of being integrated into most browsers. Additionally, encoders and decoders are still being optimized for performance, and hardware encoders and decoders are still mostly in development rather than production. - [Mozilla](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs#av1)
- **VP9/WebM**: formato menos eficiente que AV1, pero es más maduro y compatible con Safari.
- **H.264/MP4**: el formato más antiguo de la lista y compatible con versiones antiguas de Chrome, Edge, Safari, etc.
  Es incluso compatible con Internet Explorer, por lo que si queremos maximizar compatibilidad será una opción muy sólida.

La resolución elegida será **1920x1080** al ser ésta la resolución **más común** de pantalla. Podríamos codificar también en 720p y 480p como hace
Youtube, pero teniendo en cuenta que se trata de un clip de únicamente 20s, no lo vemos necesario.

En cuanto al bitrate, seguiremos la recomendación de [Youtube](https://support.google.com/youtube/answer/1722171?hl=en#zippy=%2Cbitrate)
para vídeos de 1080p 30fps: **8 Mbps**.

Como Avidemux (v2.8.0) no soporta codificación en AV1, utilizaremos únicamente **VP9** y **H.264**.
{: .notice--warning}

<div style="text-align:center">
<img src="/assets/images/mediainfomp4.png" width="250" />
<img src="/assets/images/mediainfowebm.png" width="250" />
</div>

Vemos que con el mismo bitrate, **H264 genera ruido** en algunas zonas del vídeo mientras que
**con VP9 no ocurre**.

<div style="text-align:center">
<img src="/assets/images/shadowmp4.png" width="250" />
<img src="/assets/images/shadowwebm.png" width="250" />
</div>

Además de ofrecer un **rango de colores más amplio**:

<div style="text-align:center">
<img src="/assets/images/colorwebm.png" width="300" />
<img src="/assets/images/colormp4.png" width="300" />
</div>

### Tarea 1.2: Crear web. Características del formato HTML5.

El formato HTML5 nos otorga más poder de [personalización](https://developer.mozilla.org/en-US/docs/Web/Guide/Audio_and_video_delivery/cross_browser_video_player) sobre la interfaz de usuario
comparado con servicios como Youtube, pero a cambio, deberemos tener en cuenta manualmente
los siguientes puntos:

- **Soporte de navegadores** de las distintas funcionalidades y formatos de vídeo.
- **Servidor o CDN** donde alojaremos el vídeo.
- Posibilidad de **ofrecer distintas calidades** dependiendo de la banda ancha del usuario.
- Cualquier usuario podrá **descargar** el vídeo visualizado.

### Tarea 1.3: Publicar fichero .html y valorar experiencia de usuario.

<center>
  <video width="100%" controls loop muted autoplay>
    <source src="/assets/videos/video.webm" type="video/webm">
    <source src="/assets/videos/video.mp4" type="video/mp4">
    <track kind="captions" srclang="es" src="/assets/videos/captions-es.vtt" default>
  El navegador no soporta el tag de video.
  </video>
</center>

A nivel de experiencia de usuario, el vídeo **no tiene retraso** de visualización y la calidad es
**buena**. Un detalle a tener en cuenta es que el **diseño del reproductor es dependiente** del navegador
que se esté usando.

Existen más funcionalidades que se pueden añadir al vídeo a través de la [Media API](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement) de Javascript.
Pero esto se sale un poco del alcance de la práctica.
{: .notice--info}

### Tarea 1.4: Añade funcionalidad al vídeo mediante los diferentes atributos del tag \<video\>.

```html
<video width="100%" controls loop muted autoplay>
  <source src="/assets/videos/video.webm" type="video/webm" />
  <source src="/assets/videos/video.mp4" type="video/mp4" />
  <track
    kind="captions"
    srclang="es"
    src="/assets/videos/captions-es.vtt"
    default
  />
  El navegador no soporta el tag de video.
</video>
```

- Añadido controles: `controls`
- Activada la reproducción automática y sin volumen: `autoplay` y `muted`
- La reproducción en bucle: `loop`
- Añadido subtítulos: `track` apuntando a un archivo vtt.

### Tarea 1.5: Indagar sobre los servicios _adicionales_ que ofrecen los CDN para vídeos.

Vemos que las principales plataformas cloud ofrecen servicios muy similares,
todas permiten transcodificar y analizar vídeos mediante Inteligencia Artificial.

- **AWS**:
  - [MediaConvert](https://aws.amazon.com/es/mediaconvert/) permite **transcodificar** vídeos para distintas pantallas.
  - [Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/what-is.html) analiza imágenes o vídeos para **detectar objetos**, **personas**, **texto**, escenas o **actividades**, se puede reentrenar con etiquetas personalizadas.
  - [Transcribe](https://docs.aws.amazon.com/transcribe/latest/dg/what-is.html) utiliza reconocimiento de voz para **generar subtítulos**.
  - [MediaTailor](https://aws.amazon.com/es/mediatailor/) permite insertar **anuncios personalizados** en los vídeos.
- **Google**:
  - [Transcoder](https://cloud.google.com/transcoder/docs/concepts/overview) **transcodifica** vídeos, genera **miniaturas** y normaliza el volumen del audio.
  - [Video AI](https://cloud.google.com/video-intelligence) engloba las funcionalidades relacionadas con **IA** (detección de cambios de planos, detección y seguimiento de objetos, etc).
- **Azure**:
  - [Media services](https://azure.microsoft.com/es-es/services/media-services/#overview): **administración** y **transformación** de contenido.
  - [Video Indexer](https://azure.microsoft.com/es-es/services/video-indexer): **análisis** de audio y vídeo.
- **IBM**:
  - [Video Streaming](https://www.ibm.com/products/video-streaming), para **gestionar** vídeos en streaming y bajo demanda.
  - [Watson Captioning Live](https://www.ibm.com/products/watson-captioning-live), para **generar subtítulos**.
  - [Enterprise Video Streaming](https://www.ibm.com/products/enterprise-video-streaming), para restringir contenidos y **realizar búsquedas** mediantes etiquetas generadas por IA.

Por otro lado, los CDNs más pequeños como [Cloudflare](https://www.cloudflare.com/products/cloudflare-stream/)
y [Fastly](https://www.fastly.com/es/products/streaming-media) **no suelen disponer de servicios de inteligencia artificial**,
pero suelen ser más baratos y fáciles de usar.
{: .notice--info}

## Ejercicio 2: Streaming de un vídeo

### Tarea 2.1: Generar un clip de vídeo y codificar con Avidemux (clip01).

Clip en cuestión:

<center>
<video width="100%" controls muted>
<source src="/assets/videos/clip01.mkv" type="video/mp4" />
Tu navegador no soporta videos mkv.
</video>
</center>

[Link](/assets/videos/clip01.mkv), en caso de que el navegador no soporte MKV incrustados.
{: .notice}

Vemos en `MediaInfo` que `clip01.mkv` está codificado en **AVC**, posee un
bitrate medio de **5000 Kbps** y el audio se encuentra en formato **MP3**.

<div style="text-align:center">
<img src="/assets/images/clip01_general.png" width="30%" />
<img src="/assets/images/clip01_video.png" width="30%" />
<img src="/assets/images/clip01_audio.png" width="30%" />
</div>

### Tarea 2.2: Configurar una emisión repetitiva y una recepción del clip01.

Al igual que le ha ocurrido al compañero David Ferrer, al transcodificar el vídeo en
nuestra máquica al formato `H.264 + MP3 (MP4)`, surgen problemas a la hora de visualizar.
Por ello, hemos emitido utilizando la opción de `H.264 + MP3 (TS)`, un contenedor
más adecuado para transmisiones en directo.

<div style="text-align:center">
<img src="/assets/images/clip01_emit_video.png" width="40%" />
<img src="/assets/images/clip01_emit_audio.png" width="40%" />
</div>

Comprobamos que los códecs son los mismos que hemos indicado y la tasa
de bits medio se encuentra `en torno a los 5000 Kbps` indicados.

<div style="text-align:center">
<img src="/assets/images/clip01.gif" width="300" />
</div>

### Tarea 2.3: Confirmar que el stream está codificado y tiene la tasa de bits adecuada.

Vemos que el formato de audio y vídeo es el seleccionado. Pero si lo comparamos
con el códec de emisión, vemos que el tipo de H.264 es diferente.

En la documentación de [microsoft](https://docs.microsoft.com/es-es/windows/win32/directshow/h-264-video-types) se detalla que `avc1` indica que se trata
de una secuencia de bits sin código de inicio y `h264` que sí. Esto es debido
a que al usar el protocolo RTP, no es necesario delimitar las unidades NAL explícitamente con códigos de inicio.

<div style="text-align:center">
<img src="/assets/images/clip01_receive_video.png" width="40%" />
<img src="/assets/images/clip01_receive_audio.png" width="40%" />
</div>

Estádisticas:

<div style="text-align:center">
<img src="/assets/images/clip01_receive_stats.png" width="300" />
</div>

### Tarea 2.4: ¿Cuántos segundos tarda en pausarse en el receptor? ¿Por qué?

Existe una demora de más o menos 1 segundo entre que se pausa en el emisor y se para en el receptor.
Además del overhead causado por la codificación y decodificación, esto
podría deberse al cacheo que realiza el receptor.

Si reducimos este valor, vemos que el retardo de pausa y reanudación disminuye.

<div style="text-align:center">
<img src="/assets/images/clip01_cache.png" width="100%" />
</div>

### Tarea 2.5: Calcula aproximadamente la compresión que el VLC de emisión está realizando al transcodificar.

Tomando el valor medio de 5000 Kbps que indicamos con anterioridad y los 1184 kbps que se muestran en el receptor:

El factor de compresión es: 5000 / 1184 = 4.2

<div style="text-align:center">
<img src="/assets/images/clip01_stats.png" width="100%" />
</div>

### Tarea 2.6: Describir el proceso de streaming mediante un CDN y estimar el coste por minuto de una transmisión dentro de una misma región de un stream HD en directo por 10.000 usuarios simultáneos.

Al igual que en ejercicios anteriores, utilizaremos Cloudflare como proveedor. El proceso es bastante simple:

- Pinchamos en Stream y seleccionamos la opción de Live Inputs.

<div style="text-align:center">
<img src="/assets/images/clip01_cloudflare_stream.png" width="100%" />
</div>

- Seleccionamos uno de los protocolos de streaming ofrecidos (RTMP o SRT).

<div style="text-align:center">
<img src="/assets/images/clip01_cloudflare.png" width="100%" />
</div>

- Introducimos la URL y la clave en un software compatible como puede ser OBS.

<div style="text-align:center">
<img src="/assets/images/clip01_obs.png" width="100%" />
</div>

- Aplicamos los cambios y comenzamos la emisión.

<div style="text-align:center">
<img src="/assets/images/clip01_streaming.png" width="100%" />
</div>

- Para restringir el acceso a la emisión, podemos requerir tokens tal y como se indica en la
  [documentación](https://developers.cloudflare.com/stream/viewing-videos/securing-your-stream/),
  además de activar el CORS y permitir el acceso únicamente de los dominios que indiquemos.

<div style="text-align:center">
<img src="/assets/images/clip01_cloudflare_video.png" width="100%" />
</div>

#### Coste

En cuanto al coste de la transmisión, a diferencia de otros CDNs como
AWS, donde la estimación es una tarea [compleja](https://docs.aws.amazon.com/solutions/latest/live-streaming/cost.html)
y cara (en torno a 25,7$ por minuto en el caso de 10.000 usuarios).

En [Cloudflare](https://www.cloudflare.com/products/cloudflare-stream/#simplified-pricing-of-cloudflare-stream) el modelo es mucho más simple, debemos pagar 5$ por cada 1.000 minutos guardados y 1$ por cada 1.000 minutos
servidos. De manera que el coste por minuto para 10.000 espectadores de cualquier región serían:

(1 / 1.000) \* 10.000 + (5 / 1.000) = 10,005$

Menos de la mitad que en el caso de AWS, además de no tener que pagar un extra
para usuarios de otras regiones.
{: .notice--info}
