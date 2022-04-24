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

[Descargar ZIP](https://drive.google.com/file/d/1NrAdgxKXFhgi6yHLNWDf3IFzRl9IJcyx/view?usp=sharing)

## Ejercicio 1: Contenido estático

<div style="text-align:center">
<iframe src="/assets/videos/asset-01.mp4" width="426" height="240"> </iframe>
</div>

### Tarea 1.1: Tipo de codificación

<div style="text-align:center">
<img src="/assets/images/asset-01.png" width="200" />
</div>

<br/>

El vídeo se encuentra codificado en **H.264**/**AVC**,
también conocido como **MPEG-4 Part 10**.

Se trata de una codificación **con pérdidas**
que hace uso del concepto de **GOP** (grupo de imágenes).

Teóricamente también se podría utilizar para codificar vídeos sin pérdida.
{: .notice}

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

<div style="text-align:center">
<img src="/assets/images/tabla_contenedores.png" width="200" />
</div>

### Tarea 1.5: Tipo primer y segundo fotograma

El primer fotograma es de tipo I, y el segundo fotograma es de tipo B.

### Tarea 1.6: Cantidad de cuadros de este tipo. Relación entre contenido y existencia de cuadros tipo I.

En este clip existen un total de **11 cuadros de tipo I**.

A simple vista es complicado diferenciar cuadros de tipo I y cuadros de tipo B o P.

### Tarea 1.7: Tipo de estructuras GOP

Dado que la distancia entre cuadros I es 29, deducimos que el valor N es 29.

En cuanto al valor M, vemos que todos los cuadros I están seguidos de uno B e
inmediatamente de un cuadro P, de manera que intuimos que el valor de M es 2.

### Tarea 1.8: GOP. Qué hace Avidemux cuando corta un clip para ensamblarlo a otro.

El problema al cortar un clip basado en GOP es que si cortamos un segmento
**[A, B)** y B **no** es un cuadro I, los cuadros siguientes que dependan de él
estarán rotos al haber perdido su referencia.

Avidemux trata este problema de dos [maneras](http://avidemux.sourceforge.net/doc/en/cutting.xml.html):

- Proporciona herramientas para que el usuario pueda cortar justo **antes** de un cuadro I.
- El modo **smart copy** mantiene el contenido de las partes no modificadas
  y **recodifica** únicamente los segmentos que hayan perdido sus cuadros de referencia.

En **lossless-cut**, un programa donde he contribuido en el pasado,
hubo problemas relacionados con este [tema](https://github.com/mifi/lossless-cut/pull/13)
debido a que en un principio se cortaba **directamente** en el cuadro elegido y
esto hacía que, en ocasiones, los primeros segundos de los clips generados **tuviesen audio pero
no vídeo**.
{: .notice--info}

### Tarea 1.9: MediaInfo. Información adicional.

MediaInfo proporciona **todos** los campos que muestra Avidemux
(Formato, tamaño, relación de aspecto, FPS, Bitrate medio y duración total).

Además contiene información adicional relacionado con los parámetros de la
codificación y el tratamiento del color.

## Ejercicio 2: Contenido dinámico

<div style="text-align:center">
<iframe src="/assets/videos/asset-02.mp4" width="426" height="240"> </iframe>
</div>

### Tarea 2.1: Calcular peso en bytes sin comprimir.

Utilizando la información de MediaInfo:

- Píxeles por cuadro: 1920 x 1080 = 2.073.600
- Cuadros/segundo: 30,109
- Duración: 10,263 segundos
- Peso en bytes: 2.073.600 x 3 bytes x 10,263 seg x 30,109 fps = 1.922.281.115,67 bytes (1,922 GB)

### Tarea 2.2: Contrastar con información dada por MediaInfo

En MediaInfo vemos que el archivo pesa 24.8 MiB = 26.004.684,8 bytes

### Tarea 2.3: Factor de compresión

El factor de compresión es por tanto:

1.922.281.115,67 / 26.004.684,8 = 73.92

Mucho menor que el factor de compresión calculado para el asset-01 (163,26).

### Tarea 2.4: Repetir tareas 1.5, 1.6 y 1.7

El primer fotograma es de tipo I, y el segundo fotograma es de tipo B.

En este clip existen un total de 11 cuadros de tipo I, al igual que el asset-01.

Sin embargo, aquí se ve claramente la mejora de calidad de la imagen en cuadros de tipo I.

<div style="text-align:center">
<img src="/assets/images/cuadro_i.png" width="250" />
<img src="/assets/images/cuadro_p.png" width="250" />
</div>

<br/>

Dado que la distancia entre cuadros I es 29, deducimos que el valor N es 29,
en cuanto al valor M, vemos que todos los cuadros I están seguidos por cuadros B + P o directamente P.
De manera que el valor de M debe ser también 2.

### Tarea 2.5: Diferencias entre asset-01 y asset-02.

- La diferencia de peso / factor de compresión entre asset-01 y asset-02 se debe
  a que en el primer vídeo, al ser estático, los cuadros de tipo P y B son prácticamente
  nulos y ocupan muy poco.
- Por otro lado, también vemos que en el segundo clip existen más cuadros de tipo P y
  menos cuadros de tipo B comparado con el primer clip. Entendemos que el codificador
  tomó dicha decisión para no reducir demasiado la calidad del video debido al movimiento
  y reducir el overhead de codificación.

## Ejercicio 3: Codificación en baja calidad

### Tarea 3.1: Formato de codificación DVD y Blu-ray

Los DVD aceptan tanto **H.262/MPEG-2** como MPEG-1 Part 2.
Generalmente están en formato MPEG-2 siguiendo uno de los siguientes estándares:

- **NTSC**: Usado en EE.UU., Canadá y Japón. 720 x 480 (29.97 fps)
- **PAL**: Resto del mundo. 720 x 576 (25 fps)

En caso de que el cliente hubiese pedido Blu-Ray, hubiesemos optado por el
formato original de asset-02, ya que este es compatible (MPEG-4 AVC 1920x1080 30fps) y
nos ahorraríamos tener que recodificar el clip.

### Tarea 3.2: ¿Cuál de los dos filtros crees que sería más adecuado si tu clip fuese de 30 fps?

Para mantener la duración seleccionamos la opción de **remuestrear los FPS**.

### Tarea 3.3: Factor de compresión

[pec2_dvd.mpg](/assets/videos/pec2_dvd.mpg)

El factor de compresión es:

1.922.281.115,67 / 12.052.480 = 159.492578761

En el apartado 2.3 obtuvimos 73.92, este aumento se debe a que
hemos reducido el número de píxeles, fps y bitrate.

### Tarea 3.4: Campos Número de fotogramas B y Tamaño del GOP para M=2 y N=6.

Para obtener M=2 y N=6:

- **M**: el número de fotogramas B debe ser 1, esto aseguraría que entre un fotograma I
  y el siguiente P/I haya como mucho un único fotograma B (distancia = 2).
- **N**: el tamaño del GOP debe ser 6, para que la distancia entre fotogramas I sea
  exactamente 6.

### Tarea 3.5: Factor de compresión

[pec2_dvd_m2_n6.mpg](/assets/videos/pec2_dvd_m2_n6.mpg)

El factor de compresión es:

1.922.281.115,67 / 12.083.200 = 159.087

Es similar al calculado en el apartado 3.3, la pequeña diferencia
podría deberse al aumento de cuadros I al disminuir el tamaño del GOP.

## Ejercicio 4: Codificación en Blu Ray y codificación en 4K

### Tarea 4.1: HEVC. Novedades H.265/HEVC respecto a H.264/MPEG-4 AVC.

HEVC son las siglas de High Efficiency Video Coding, es también conocido como
H.265 o MPEG-H Part 2.

Según [ADSLZone](https://www.adslzone.net/reportajes/tecnologia/que-es-hevc/)
las principales mejoras sobre H.264 son:

- El límite de resolución pasa de 4K 60fps a 8K 300fps.
- Proporciona mejor calidad al mismo bitrate gracias a la utilización de
  **unidades de codificación** en lugar de macrobloques (a cambio, la etapa de decodificación es mucho más costosa).
- Mejora el seguimiento del movimiento al utilizar vectores de movimiento de hasta 16 bits.

### Tarea 4.2: Niveles y los perfiles de H.265/HEVC, comparar con H.264/MPEG-4 AVC.

En [Wikipedia](https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding#Profiles) podemos ver los distintos perfiles y niveles que se han introducido cada versión del estándar H.265/HEVC:

- Versión 1: Main, Main 10 y Main Still Picture.
- Versión 2: Monochrome, Monochrome 12, Monochrome 16, Main 12, Main 4:2:2 10, Main 4:2:2 12, Main 4:4:4, Main 4:4:4 10, Main 4:4:4 12, Monochrome 12 Intra, Monochrome 16 Intra, Main 12 Intra, Main 4:2:2 10 Intra, Main 4:2:2 12 Intra, Main 4:4:4 Intra, Main 4:4:4 10 Intra, Main 4:4:4 12 Intra, Main 4:4:4 16 Intra, Main 4:4:4 Still Picture, Main 4:4:4 16 Still Picture, High Throughput 4:4:4 16 Intra, Scalable Main, Scalable Main 10 y Multiview Main.
- Versión 3: Screen-Extended Main, Screen-Extended Main 10, Screen-Extended Main 4:4:4, Screen-Extended Main 4:4:4 10, Screen-Extended High Throughput 4:4:4, Screen-Extended High Throughput 4:4:4 10, Screen-Extended High Throughput 4:4:4 14, High Throughput 4:4:4, High Throughput 4:4:4 10, High Throughput 4:4:4 14, Scalable Monochrome, Scalable Monochrome 12, Scalable Monochrome 16 y Scalable Main 4:4:4

### Tarea 4.3: Nivel de codificación.

El nivel se indica en el selector de de IDC Level.

### Tarea 4.4: Perfiles en Avidemux para H.264 y H.265.

- Para H.264 se pueden seleccionar los perfiles: baseline, main, high, high10, high422 y high444.
- Para H.265: main y mainstillpicture.

En nuestro caso también podemos utilizar la GPU y configurar como salida
NVIDIA H264 (perfiles baseline, main, high) y NVIDIA HECV (main, main10).
{: .notice--info}

### Tarea 4.5: Factor de compresión con respecto el fichero asset-02.

[hevc-80.mkv](/assets/videos/hevc-80.mkv)

Peso del fichero (reportado por Windows): 25.983.830 bytes
Duración (reportado por MediaInfo): 10,263 s
Bitrate: 25.983.830 x 8 / 10,263 = 20254373,9647 bps = 20254,374 kbit/s

Nuevo bitrate = 20254,374 kbit/s \* 0.8 = 16203.4992 kbit/s

El factor de compresión es:
25.983.830 / 21.749.074 = 1,1947

Su inversa es 0.83, es decir, el segundo fichero pesa en torno un 83% del primer fichero.
{: .notice--info}

### Tarea 4.6: Factor de compresión con respecto a un hipotético clip sin comprimir.

Utilizando el valor calculado en el ejercicio 2.2:
1.922.281.115,67 / 21.749.074 = 88.3845

### Tarea 4.7: 60% de velocidad con modo Tasa de bits media o Average Bit Rate (Two Pass)

[hevc-60.mkv](/assets/videos/hevc-60.mkv)

Nuevo bitrate = 20254,374 kbit/s \* 0.6 = 12152.6244 kbit/s

Con respecto a asset-02:
25.983.830 / 15.529.367 = 1,673

Con respecto a un vídeo sin comprimir:
1.922.281.115,67 / 15.529.367 = 123,78

El peso ha disminuido considerablemente y la calidad no parece haber disminuido, esto
podría deberse a que permitiendo que el bit rate varíe, el codificador es más inteligente
comprimiendo más los segmentos más estáticos y menos los de mayor movimiento.

### Tarea 4.8: Efecto visual pérdidad de libertad de elección de su medida y compresión.

[hevc-60-gop-30.mkv](/assets/videos/hevc-60-gop-30.mkv)

El tamaño del archivo ha aumentado muy ligeramente (quizás por el aumento de cuadros I)
y la calidad ha disminuido.

### Tarea 4.9: Comparar los 3 casos. Variación GOP: calidad visual y compresión.

[hevc-60-gop-10.mkv](/assets/videos/hevc-60-gop-10.mkv)

Es algo difícil de comparar, pero exportando el frame inicial vemos que cuando disminuimos el tamaño del GOP:

- La calidad visual disminuye debido a que necesitamos guardar más cuadros I (a expensas del número
  de cuadros P y B, más eficientes) manteniendo el mismo bit rate.
- Los tamaños de los ficheros son similares debido a que el bit rate medio es el mismo.

<div style="text-align:center">
<img src="/assets/images/hevc-60.png" width="250" />
<img src="/assets/images/hevc-60-gop-30.png" width="250" />
<img src="/assets/images/hevc-60-gop-10.png" width="250" />
</div>

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
