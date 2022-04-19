---
layout: single
permalink: /pec-2-plataformas/
title: "Plataformas de publicación y distribución: PEC 2"
author_profile: true
toc: true
disallow: true
---

Esta página no se encuentra indexada en Google
{: .notice--warning}

## Ejercicio 1: Contenido estático

### Tarea 1.1: Tipo de codificación

El vídeo se encuentra codificado en **H.264**/**AVC**,
también conocido como **MPEG-4 Part 10**.

Se trata de una codificación **con pérdidas**
(aunque teóricamente también se podría generar sin pérdida)
que hace uso del concepto de **GOP** (grupo de imágenes).

### Tarea 1.2: Calcular peso en bytes sin comprimir.

- Píxeles por cuadro: 1920 x 1080 = **2.073.600**
- Cuadros/segundo: **30,108**
- Duración: **10,229** segundos
- Peso en bytes: 2.073.600 x 3 bytes x 10,229 seg x 30,108 fps = **1.915.849.212,83** bytes (1,915 GB)

### Tarea 1.3: Factor de compresión

Windows reporta un tamaño de archivo de **11.734.889** bytes.

Por tanto el factor de compresión es: 1.915.849.212,83 / 11.734.889 = **163,26**

Esto implica un ahorro de espacio del 99,38%.

El factor real es algo mayor, ya que estamos ignorando el peso de la pista de audio.
{: .notice--info}

### Tarea 1.4: Tipo de contenedor

El contenedor es **MP4**, de manera que concuerda con los contenedores
compatibles con MPEG-4 AVC que se muestran en la tabla.

{% include figure image_path="/assets/images/tabla_contenedores.png" %}

### Tarea 1.5: Tipo primer y segundo fotograma

El primer fotograma es de tipo I, y el segundo fotograma es de tipo B.

### Tarea 1.6: Cantidad de cuadros de este tipo. Relación entre contenido y existencia de cuadros tipo I.

En este clip existen un total de 11 cuadros de tipo I.
A simple vista es complicado diferenciar cuadros de tipo I y cuadros de tipo B o P.

### Tarea 1.7: Tipo de estructuras GOP

Dado que la distancia entre cuadros I es 29, deducimos que el valor N es 29,
en cuanto al valor M, vemos que todos los cuadros I están seguidos de uno B e
inmediatamente de un cuadro P, de manera que intuimos que el valor de M es 2.

### Tarea 1.8: GOP. Qué hace Avidemux cuando corta un clip para ensamblarlo a otro.

El problema al cortar un clip basado en GOP es que si cortamos un segmento
[A, B) y B no es un cuadro I, los cuadros siguientes que dependan de él
estarán rotos al haber perdido su referencia.

Avidemux trata este problema de dos [maneras](http://avidemux.sourceforge.net/doc/en/cutting.xml.html):

- Proporciona herramientas para que el usuario pueda cortar justo antes de un cuadro I.
- El modo smart copy mantiene el contenido de las partes no modificadas
  y recodifica únicamente los segmentos que hayan perdido sus cuadros de referencia.

En **lossless-cut**, un programa donde he contribuido en el pasado,
hubo problemas relacionados con este [tema](https://github.com/mifi/lossless-cut/pull/13)
debido a que en un principio se cortaba **directamente** en el cuadro elegido y
esto hacía que los primeros segundos de los clips generados **tuviesen audio pero
no vídeo**.
{: .notice--info}

### Tarea 1.9: MediaInfo. Información adicional.

MediaInfo proporciona **todos** los campos que muestra Avidemux
(Formato, tamaño, relación de aspecto, FPS, Bitrate medio y duración total).

Además contiene información adicional relacionado con los parámetros de la
codificación y el tratamiento del color.

## Ejercicio 2: contenido dinámico

### Tarea 2.1: Calcular peso en bytes sin comprimir.

Utilizando la información de MediaInfo:

- Píxeles por cuadro: 1920 x 1080 = 2.073.600
- Cuadros/segundo: 30,109
- Duración: 10,263 segundos
- Peso en bytes: 2.073.600 x 3 bytes x 10,263 seg x 30,109 fps = 1.922.281.115,67 bytes (1,922 GB)

### Tarea 2.2: Contrastar con información dada por MediaInfo

En MediaInfo vemos que el archivo pesa 24.8 MiB = 26.004.684,8 bytes

### Tarea 2.3: Factor de compresión

De manera que el factor de compresión es:

1.922.281.115,67 / 26.004.684,8 = 73.92

Mucho menor que el factor de compresión calculado para el asset-01 (163,26).

### Tarea 2.4: Repetir tareas 1.5, 1.6 y 1.7

El primer fotograma es de tipo I, y el segundo fotograma es de tipo B.

En este clip existen un total de 11 cuadros de tipo I, al igual que el asset-01.

Por otro lado, aquí se ve claramente la mejora de calidad de la imagen en cuadros de tipo I.

Dado que la distancia entre cuadros I es 29, deducimos que el valor N es 29,
en cuanto al valor M, vemos que todos los cuadros I están seguidos por cuadros B + P o directamente P.
De manera que intuimos que el valor de M es también 2.

### Tarea 2.5: Diferencias entre asset-01 y asset-02.

- La diferencia de peso / factor de compresión entre asset-01 y asset-02 es debido
  a que en el primer vídeo, al ser estático, los cuadros de tipo P y B son prácticamente
  nulos y ocupan muy poco.
- Por otro lado, también vemos que en el segundo clip existen más cuadros de tipo P y
  menos cuadros de tipo B comparado con el primer clip. Entendemos que el codificador
  tomó dicha decisión para no reducir demasiado la calidad del video debido al movimiento
  y reducir el overhead de codificación.

## Ejercicio 3: Codificación en baja calidad

### Tarea 3.1: Formato de codificación DVD y Blu-ray

Los DVD aceptan tanto H.262/MPEG-2 como MPEG-1 Part 2.
Generalmente están en formato MPEG-2 siguiendo uno de los siguientes estándares:

- NTSC: Usado en EE.UU., Canadá y Japón. 720 x 480 (29.97 fps)
- PAL: Resto del mundo. 720 x 576 (25 fps)

En caso de que el cliente hubiese pedido Blu-Ray, hubiesemos optado por
formato original de asset-02, ya que este es compatible (MPEG-4 AVC 1920x1080 30fps) y
nos ahorraríamos tener que recodificar el clip.

### Tarea 3.2: ¿Cuál de los dos filtros crees que sería más adecuado si tu clip fuese de 30 fps?

Para mantener la duración seleccionamos la opción de **remuestrear los FPS**.

### Tarea 3.3: Factor de compresión

TODO

### Tarea 3.4: Campos Número de fotogramas B y Tamaño del GOP para M=2 y N=6.

Para obtener M=2 y N=6:

- M: el número de fotogramas B debe ser 1, de manera que entre un fotograma I
  y el siguiente P/I habrá como mucho 1 fotograma B (distancia = 2).
- N: el tamaño del GOP debe ser 6, para que la distancia entre fotogramas I sea
  exactamente 6.

### Tarea 3.5: Factor de compresión

TODO

## Ejercicio 4: Codificación en Blu Ray y codificación en 4K

### Tarea 4.1: HEVC. Novedades H.265/HEVC respecto a H.264/MPEG-4 AVC.

TODO

### Tarea 4.2: Nivels y los perfiles de H.265/HEVC, comparar con H.264/MPEG-4 AVC.

TODO

### Tarea 4.3: Nivel de codificación.

TODO

### Tarea 4.4: Perfiles en Avidemux para H.264 y H.265.

TODO

### Tarea 4.5: Factor de compresión con respecto el fichero asset-02.

TODO

### Tarea 4.6: Factor de compresión con respecto a un hipotético clip sin comprimir.

TODO

### Tarea 4.7: 60% de velocidad con modo Tasa de bits media o Average Bit Rate (Two Pass)

TODO

### Tarea 4.8: Efecto visual pérdidad de libertad de elección de su medida y compresión.

TODO

### Tarea 4.9: Comparar los 3 casos. Variación GOP: calidad visual y compresión.

TODO

## Ejercicio 5: Codificación de vídeo mediante un CDN

En este apartado analizaremos el producto
[Cloudflare Stream](https://www.cloudflare.com/products/cloudflare-stream/),
principalmente debido a la **sencillez** de su modelo de precios y uso.

En proveedores más clásicos como [AWS](https://aws.amazon.com/es/cloudfront/streaming/),
debemos utilizar una combinación de **distintos servicios** para conseguir un flujo
completo desde la subida del vídeo hasta la visualización del mismo por un usuario.

![aws](https://d1.awsstatic.com/products/cloudfront/VOD%20Architecture%20CloudFront.aa3cb2ec3a8660b42f90072c60672a52d9c357a6.png)

Como vemos en esta imagen, el flujo en Amazon es el siguiente:

1. Subimos el vídeo a **Amazon S3**, un servicio de almacenamiento de objetos.
2. Esta subida genera un evento que lanza un **AWS Lambda** para codificar el vídeo en
   los formatos que deseemos mediante **AWS Elemental MediaConvert**.
3. Una vez termine de codificarse, se generará otro evento para que el vídeo sea
   guardado en el S3 de destino (hay que tener cuidado de no crear un bucle infinito
   usando el mismo bucket de entrada).
4. Por último, debemos configurar **CloudFront** para permitir la distribución
   global del contenido.

Aunque exista una [guía](https://docs.aws.amazon.com/solutions/latest/video-on-demand-on-aws-foundations/welcome.html)
exhaustiva de todo el proceso, acompañado por un template de **CloudFormation** (infraestructura como código),
el proceso puede resultar confuso y, por otro lado, no es sencillo **estimar el
coste** de todo el proceso.

Por ello, CloudFlare [introduce](https://blog.cloudflare.com/introducing-cloudflare-stream/)
en 2017 una solución "end-to-end" para todo el proceso:

- El modelo de coste es sencillo: el cliente paga mensualmente 1$ por cada 1.000
  minutos visualizados y 5$ por cada 1.000 minutos de contenido persistido en la plataforma.
- **Codificación [automática](https://developers.cloudflare.com/stream/faq/#what-formats-and-quality-levels-are-delivered-through-cloudflare-stream)** en diferentes resoluciones y bit rates.
- **[Reproductor](https://developers.cloudflare.com/stream/viewing-videos/using-the-stream-player/) personalizable** e incrustable.
- Soporte de **[Streaming adaptativo](https://developers.cloudflare.com/stream/viewing-videos/using-own-player/)**.
- **Securización** utilizando [signed URLs](https://developers.cloudflare.com/stream/viewing-videos/securing-your-stream/).
- **Descarga** en formato [MP4](https://developers.cloudflare.com/stream/viewing-videos/download-videos/).

En el aspecto más técnico, actualmente Cloudflare Stream:

- Utiliza el codec **H264** y su resolución varía entre 360p y 1080p, el cliente
  se abstrae de este punto, por lo que podría cambiar en un futuro si surgiese
  un mejor formato.
- **No** tiene aún soporte para vídeos con **múltiples pistas de audio**.
- **No** persiste el **vídeo original** en sus servidores.
- Proporciona streaming adaptativo mediante HTTP Streaming, en concreto **DASH y HLS**.
- Permite subidas de vídeo en formato: MP4, MKV, MOV, AVI, FLV, MPEG-2 TS, MPEG-2 PS, MXF, LXF, GXF, 3GP, WebM, MPG y QuickTime.
- Si el vídeo supera los 70 fps es **remuestreado**.

Página de [preguntas frequentes](https://developers.cloudflare.com/stream/faq)
{: .notice--info}