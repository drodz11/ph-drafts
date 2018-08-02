---
title: |
    Introducción a FFmpeg
authors:
- David Rodriguez
translators:
- David Rodriguez
- Sebastian Fiori
date: 2018-05-02
reviewers:
layout: lesson
---

# Introducción
Historicamente, las Humanidades Digitales, como una disciplina, se han enfocado casi exclusivamente en el analisis de fuentes textuales a través de métodos computacionales (Hockey, 2004). Sin embargo, hay un interés creciente en el campo de la utilización de métodos computacionales para el analisis de materiales audiovisuales de herencia cultural como se indica por la creación de el [Alianza de Organizaciones de Humanidades Digitales Grupo de Interés Especial:
Materiales audiovisuales en Humanidades Digitales](https://avindhsig.wordpress.com/) y el [el aumento de las presentaciones relacionadas con temas audiovisuales en la conferencia global de AOHD](https://figshare.com/articles/AV_in_DH_State_of_the_Field/5680114) en los años pasados. Como estados Erik Champion, "La audiencia de HD no siempre está enfocada en la literatura o está interesada en las formas tradicionales de alfabetización" y la aplicación de metodologías digitales para estudiar cultura audiovisual es un emocionante y emergente faceta de Humanidades Digitales (Champion, 2017). Hay muchos valoroso, gratuita, y código abierto herramientas y recursos disponibles para aquellos interesados en trabajar con materiales audiovisuales (por ejemplo, el Programming Historian tutorial [Editar Audio con Audacity](https://programminghistorian.org/es/lecciones/editar-audio-con-audacity)) y este tutorial presentará otro: FFmpeg.

[FFmpeg](https://www.ffmpeg.org/) es el marco multimedia de código abierto líder para transcodificar, editar, filtrar, y reproducir casi cualquier tipo de formato audiovisual digital. Muchos programs comunes y sitios web usan FFmpeg para leer y escribir archivos audiovisuales incluso VLC, Google Chrome, YouTube y [muchos más](https://trac.ffmpeg.org/wiki/Projects). Además de ser una herramienta de software y desarrollo de Internet, FFmpeg se puede usar en la interfaz de línea de comandos para realizar muchas tareas comúnes, complejas, e importantes relacionadas con la preservación, reproducción de archivos, y visualización y recuperación de metadatos. Como tal, FFmpeg es una herramienta increíblemente valiosa para los humanistas digitales que trabajan con datos audiovisuales. El conocimiento del marco permite a los investigadores manipular materiales audiovisuales para satisfacer sus necesidades con una solución gratuita de código abierto con mucha de la misma funcionalidad que los costosos software de audio y vídeo.

Aunque es útil tener cierta familiaridad con las herramientas de Bash, la interfaz de línea de comandos, o otros lenguajes de programación, este conocimiento previo no es critico. Si le interesa aprender más sobre estas habilidades, revise [Introducción a la línea de comandos en Bash](https://programminghistorian.org/es/lecciones/introduccion-a-bash) o [Introducción a la línea de comandos de Windows con PowerShell](https://programminghistorian.org/es/lecciones/introduccion-a-powershell). Adicionalmente, una entendimiento básica de [contenedores](https://es.wikipedia.org/wiki/Formato_contenedor) y [códecs](https://es.wikipedia.org/wiki/C%C3%B3dec) audiovisuales también será útil para entender qué hace FFmpeg y cómo funciona.

# Objectivos de aprendizaje
* Aprender a instalar FFmpeg en su computadora o usar una versión en el navegador de Internet
* Comprender la estructura básica y sintaxis de los comandos de FFmpeg
* Aprender varios comandos útiles
* Introducir recursos para mayor exploración y experimentación

# Cómo instalar FFmpeg
La instalación de FFmpeg es posiblemente la parte más difícil de usar FFmpeg. Felizmente, hay algunas guías útiles y recursos disponibles para instalar el marco para cada tipo de sistema operativo.

* **Nota**: Nuevas versiones de FFmpeg son lanzadas aproximadamente cada seis meses. Para mantenerse al tanto de las nuevas versiones, siga FFmpeg en [Twitter](https://twitter.com/FFmpeg) o en [el sitio web](https://www.ffmpeg.org/index.html#news)

## Para usuarios de Mac OS
La opción más simple es usar un administrador de paquetes como [Homebrew](https://brew.sh/) para instalar FFmpeg y asegurar que permanezca en la versión más reciente. Para completar este tipo de instalación, siga estos pasos:
* Instale Homebrew de acuerdo a las instrucctiones en el sitio web
* Ejecute `brew install ffmpeg` en su [Terminal](https://es.wikipedia.org/wiki/Terminal_(OS_X)) para comenzar una instalación básica
  * **Nota**: Generalmente, es recomendado instalar FFmpeg con opciones adicionales a las incluidas en la instalación básica. Incluir opciones adicionales proporcionará accesso a más herramientas y funcionalidades de FFmpeg. [La Guía de Instalación de Apple de Reto Kromer](https://avpres.net/FFmpeg/install_Apple.html) proporciona un buen conjunto de opciones adicionales:
  `brew install ffmpeg --with-sdl2 --with-freetype --with-openjpeg --with-x265 --with-rubberband --with-tesseract`
  * Para una explicación de estas opciones adicionales, revise [La Guía FFmpeg de Ashley Blewer](https://training.ashleyblewer.com/presentations/ffmpeg.html#10).
* Para actualizar su instalación a la versión más reciente, ejecute:
`brew update && brew upgrade ffmpeg`
* Para más opciones de instalación para Mac OS, revise [La Guía Compilacion de FFmpeg para Mac OS](https://trac.ffmpeg.org/wiki/CompilationGuide/macOS).

## Para usarios de Windows
Usarios de Windows pueden usar el adminstratdor de paquetas [Chocolately](https://chocolatey.org/) para instalar y mantener FFmpeg. [La Guía de Instalación de Windows de Reto Kromer](https://avpres.net/FFmpeg/install_Windows.html) proporciona todo la información necesaria para usar Chocolately o construir el marco a partir del código fuente.

## Para usarios de Linux
[Linuxbrew](http://linuxbrew.sh/), una programa similar a Homebrew, se puede usar para instalar y mantener FFmepg en Linux. Reto Kromer también proporciona una guía útil, [La Guía de Instalación de Linux](https://avpres.net/FFmpeg/install_Linux.html), que es similar a la instalación en Mac OS.

## Otras Recursos para Instalar

* [Paquetas de descargar](https://www.ffmpeg.org/download.html)
* [La Guía Compilacion de FFmpeg](https://trac.ffmpeg.org/wiki/CompilationGuide)

## Probando La Instalación
* Para asegurarse de que FFmpeg se haya instalado correctamente, ejecute:
`ffmpeg -version`
* Si ve una producción larga de información, la instalación fue exitosa. Debe ser similar a lo siguiente:

`ffmpeg version 3.4 Copyright (c) 2000-2017 the FFmpeg developers
built with Apple LLVM version 9.0.0 (clang-900.0.38)
configuration: --prefix=/usr/local/Cellar/ffmpeg/3.4 --enable-shared --enable-pthreads --enable-version3 --enable-hardcoded-tables --enable-avresample --cc=clang --host-cflags= --host-ldflags= --enable-gpl --enable-ffplay --enable-libfreetype --enable-libmp3lame --enable-librubberband --enable-libtesseract --enable-libx264 --enable-libx265 --enable-libxvid --enable-opencl --enable-vídeotoolbox --disable-lzma --enable-libopenjpeg --disable-decoder=jpeg2000 --extra-cflags=-I/usr/local/Cellar/openjpeg/2.3.0/include/openjpeg-2.3
libavutil 55. 78.100 / 55. 78.100
libavcodec 57.107.100 / 57.107.100
libavformat 57. 83.100 / 57. 83.100
libavdevice 57. 10.100 / 57. 10.100
libavfilter 6.107.100 / 6.107.100
libavresample 3. 7. 0 / 3. 7. 0
libswscale 4. 8.100 / 4. 8.100
libswresample 2. 9.100 / 2. 9.100
libpostproc 54. 7.100 / 54. 7.100`

* Si ve algo como `-bash: ffmpeg: command not found`, algo ha ido mal.

## Usando FFmpeg en El Navegador de Internet
Si no quiere instalar FFmepg en su compudatora pero le gustaría familiarizarse con el marco y usarlo en la interfaz de línea de comandos, [vídeoconverter.js](https://bgrins.github.io/videoconverter.js/demo/)
proporciona un método para ejecutar los comandos FFmpeg en su navegador de Internet.
  **Nota**: Este recurso opera en una versión más vieja de FFmpeg y posiblemente no tenga todas las características de la versión más reciente.

# La Estructura Básica y Sintaxis de Los Comandos FFmpeg
El comando básico tiene cuatro partes:

`[Símbolo del Sistema] [Archivo de Entrada] [Banderas/Acciónes] [Archivo de Salida]`

* Cada comando comenzará con un símbolo del sistema. Dependiendo del uso, esto será `ffmpeg` (cambiar archivos), `ffprobe` (generar metadatos de archivos), or `ffplay` (reproducir archivos).
* Los archivos de entradas son los archivos que están siendo leídos, editatos, o examinados. Los archivos de salida son los archivos creados por el comando o los informes creados por los commandos de `ffprobe`.
* Las banderas y acciónes son las cosas que usted le está diciendo a FFmpeg que haga con los archivos de entradas. La mayoría de los comandos contendrán múltiples banderas y acciónes de complejidad variable.

Escrito genéricamente, el comando básico se parece a lo siguiente:

 `ffmpeg -i /ruta_de_archivo/archivo_de_entrada.ext -bandera algo_acción /ruta_de_archivo/archivo_de_salida.ext`

A continuación, examinaremos algunos ejemplos de varios comandos diferentes que usan esta estructura y sintaxis. Adicionalmente, estos comandos demostrarán algunas de las características más útiles de FFmpeg.

# Ejemplos de Comandos
Los siguientes ejemplos están escritos con nombres de archivos genéricos como `entrada_archivo.ext`. En la práctica real, deberá escribir nombres, extensiones, y rutas de archivos completos de todos archivos de entrads y salidas. Puede usar estos comandos con cualquier archivo de vídeo, o descargar una muestra de [vídeoconverter.js](https://bgrins.github.io/videoconverter.js/demo/bigbuckbunny.webm). Este vídeo ha sido provisto por [The Blender Foundation](https://www.blender.org/foundation/) bajo una licencia [Creative Commons Attribution 3.0](https://creativecommons.org/licenses/by/3.0/).
Además, cada ejemplo proporciona una breve explicación de cada parte del comando.

## Cambiar el Contenedor (Volver a envolver)
En este ejemplo, empecemos con un archivo de entrada con una extensión (contenedor) `.mp4` y crear un nuevo archivo de salida "vuelto a envolver" con una contenedor`.mov`. Puede ser necessario a cambiar contenedors dependiendo en su sistema operativo y la compatibilidad de otras software de audiovisual que está usando, especialmente si quiere mantener la codificación original (códec) de el archivo de entrada (vamos a examinar como cambiar el códec o "transcodificar" en el siguiente ejemplo).

Para cambiar el contenedor/la extensión de un archivo:

`ffmpeg -i entrada_archivo.mp4 -c copy -map 0 salida_archivo.mov`

* `ffmpeg` = comienza el comando
* `-i entrada_archivo.mp4` = la ruta y nombre del archivo de entrada
* `-c copy` = copia las pistas directamente, sin recodificar
* `-map 0` = mapear todas las pistas en el archivo de entrada en el archivo de salida. Esto es necesario, especialmente si el archivo tiene pistas múltiples de audio o subtítulos.
* `salida_archivo.mov` = la ruta y el nombre del archivo de salida. Note que aquí es donde se define el nuevo contenedor.

## Cambiar el Contenedor y Códec (Transcodificar)
En este ejemplo, recodificamos (transcodificar) un nuevo archivo de salida a una determinada especificación. A transcodificar archivos audiovisuales es un prodecimiento muy común y a menudo necesario para compatibilidad o si necesita a crear versiones sustituos o derivados de un archivo. Adicionalmente, a menudo querrás o necesitarás a volver a envolver y transcodificar archivos con el mismo commando. El siguiente comando escribirá un archivo de salida de ProRes 422 LT con una extensión `.mov` y tambien recodificará la pista de audio del archivo al códec `aac`:

`ffmpeg -i entrada_archivo.mp4 -c:v prores -profile:v 1 -c:a aac -vf yadif salida_archivo.mov`

* `ffmpeg` = comienza el comando
* `-i entrada_archivo.ext` = la ruta y el nombre del archivo de entrada
* `-c:v prores` = copia la pista de vídeo a ProRes 422
* `-profile:v 1` = indica el perfil LT de ProRes. Más sobre [los perfiles de ProRes](https://documentation.apple.com/en/finalcutpro/professionalformatsandworkflows/index.html#chapter=10%26section=2)
* `-c:a aac` = copia el códec de audio a AAC
* `-vf yadif` = usa el filtro de vídeo "yadif" para desentrelazar la imagen
* `salida_archivo.mov` = la ruta y el nombre del archivo de salida. Note que aquí es donde se define el nuevo contenedor

## Demux Audio y Vídeo (Separar audio y vídeo en diferents archivos)
"Demuxing" un archivo audiovisual simplemente significa a seperar su diferentes componentes or pistas (por ejemplo, las pistas de audio y vídeo) en sus propios archivos. Esto es útil si está interesando en examinar estos componentes discretamente o para performar algún tipo de análisis especializado.

Para separar las pistas de audio y vídeo en un archivo y crear nuevos archivos:

### Extraer Audio
Para crear un nuevo archivo de audio (sin vídeo):

`ffmpeg -i entrada_archivo.ext -c:a copy -vn salida_archivo.ext`

### Extraer vídeo
Para crear un nuevo archivo de vídeo (sin audio):

`ffmpeg -i entrada_archivo.ext -c:v copy -an salida_archivo.ext`

* `ffmpeg` = comienza el comando
* `-i entrada_archivo.ext` = la ruta y el nombre del archivo de entrada
* `-c:v copy` = copia la pista de vídeo directamente, sin recodificar
* `-c:a copy` = copia la pista de audio directamente, sin recodificar
* `-vn` = le dice a FFmpeg que ignore la pista de vídeo
* `-an` = le dice a FFmpeg que ignore la pista de audio
* `salida_archivo.ext` = la ruta y el nombre del archivo de salida

Nota que necesita especificar la extensión correcta dependiendo en el tipo del archivo de salida está creando, sin embargo el contenedor `.mp4` puede ser y se usa comúnmente para audio o video.

## Recortar Archivos
Comparablemente, puedes crear un extracto de un archivo para análisis o investigación más enfocado. Hay varios métodos para hacer esto, y los siguientes ejemplos proporcionan usas diferentes del syntaxis de FFmpeg para crear extractos.

Para crear un extracto de un archivo sin recodificar:

 `ffmpeg -i entrada_archivo.ext -ss 00:02:30 -to 00:05:00 -c copy -map 0 salida_archivo.ext`

 * `ffmpeg` = comienza el comando
 * `-i entrada_archivo.ext` = la ruta y el nombre del archivo de entrada
 * `-ss 00:02:30` = establece el punto de inicio a los 2 minutos y 30 segundos desde el inicio del archivo
 * `-to 00:05:00` = establece el punto final a los 5 minutos desde el inicio del archivo
 * `-c copy` = copia las pistas directamente, sin recodificar
 * `-map 0` = mapear todas las pistas en el archivo de entrada en el archivo de salida
 * `salida_archivo.ext` = la ruta y el nombre del archivo de salida

 Alternativamente, puede usar `-t` para asignar la duración en lugar de los puntos de incio y final:

 `ffmpeg -i entrada_archivo.ext -ss 00:02:30 -t 150 -c copy -map 0 salida_archivo.ext`

* `-ss 00:02:30 -t 150` = establece el punto de inicio a los 2 minutos y 30 segundos desde el inicio del archivo y crea un fragmento de 150 segundos de duración

Para crear un fragmento que comienza al comienzo del archivo:

`ffmpeg -i entrada_archivo.ext -t 30 -c copy -map 0 salida_archivo.ext`

* En este comando, usar `-t 30` crea un fragmento de 30 segundos que comienza en `00:00:00`

## Usando FFprobe (Generar informes de metadata)
`FFprobe` es una herramienta poderosa para extraer los metadatos de archivos audiovisuales. Esta informacion puede enviarse a varios formatos de datos estructurados (legible por máquina), incluidos `.json` y `.xml`, que se pueden usar para analisis computacional y simplemente para almacenar informacion importante sobre un archivo(s). Este ejemplo escribirá un archivo de `.json` conteniendo los metadatos técnicos del archivo de entrada.

`ffprobe -i entrada_archivo.ext -show_format -show_streams -print_format json > salida_metadata_archivo.json`

* `ffprobe` = comienza el comando
* `-i entrada_archivo.ext` = la ruta y el nombre del archivo de entrada
* `-show_format` = proporciona la información del contenedor
* `-show_streams` = proporciona la información del códec para todas las pistas
* `-print_format json` = especifica el formato del informe de los metadatos.
* `> output_metadata_file.json` = escribe un nuevo archivo de `.json` conteniendo el informe de metadatos a una ruta y un nombre de archivo específico. La extensión de archivo debe coincidir con el formato especificado por la bandera `print_format`
  * Alternativamente, puede escribe este comando sin `> output_metadata_file.json`
  y el informe se va a imprimir a `stdout`.

Para más información sobre esto comando, mire la [explicación de Reto Kromer](https://avpres.net/FFmpeg/probe_json.html)

## Usando FFplay (Reproducir archivos)
`FFplay` es útil para probar y reproducir nuevos archivos que han sido creados con los comandos `ffmpeg` o como un simple reproductor multimedia. Como veremos, se puede usar `ffplay` con para parámetros especificos y filtros. Para reproducir un archivo, ejecute:

`ffplay entrada_archivo.ext`

Este comando reproduce el archivo una vez y luego cierra la ventana de comando. Note que con los commandos `ffplay`, no necesita indicar una bandera `-i` o un archivo de salida porque la reproducción es la salida. Para repetir la reprodución indefinidamente:

`ffplay -loop 0 input_file.ext`

El número `0` se puede cambiar a cualquier número y `ffplay` reproducirá el archivo tantas veces.

## Visualizar Información Audio y Vídeo (Crear vectorscopio y forma de onda)
[La visualización de datos](https://es.wikipedia.org/wiki/Visualizaci%C3%B3n_de_datos) es un concepto familiar para los humanistas digitales. Por años, los profesionales de sonido y vídeo también han usado la visualización de datos para trabajar con contenido audiovisual. Estos tipos de visualizaciones incluyen [vectorscopios](https://es.wikipedia.org/wiki/Vectorscopio) (para visualizar la información del color del vídeo) y [formas de onda](https://en.wikipedia.org/wiki/Waveform) (para visualizar los datos de la señal de audio). Aunque este tipo de la visualización de datos no es del tipo tradicionalmente creado por el trabajo HD, FFmpeg incluye una serie de herramientas y bibliotecas que se pueden utilizar para visualizar información de sonido e imagen que potencialmente puede expandir el campo y abrir nuevas líneas de investigación crítica.

Esta sección proporciona los comandos para crear algunos tipos diferentes de visualizaciones con ejemplos de los resultados previstos. Adicionalmente, estos ejemplos son más complejos que los ejemplos anteriores de este tutorial y proporcionan más información sobre la creación de opciones complejas para los comandos de FFmpeg.

### Vectorscopio (Vídeo)
Para reproducir un vídeo con un vectorscopio:

`ffplay entrada_archivo.ext -vf "split=2[m][v], [v]vectorscope=b=0.7:m=color3:g=green[v],[m][v]overlay=x=W-w:y=H-h"`

* `ffplay` = comienza el comando
* `-i entrada_archivo.ext` = la ruta y el nombre del archivo de entrada
* `-vf` = crea un *filter-graph* para usar con las pistas
* `"` = una comilla comienzo el *filter-graph.* La información entre las comillas
 especifica los parámetros de la apariencia y posición del vectorscopio
* `split=2[m][v]` = divide la entrada en dos salidas idénticas llamadas `[m]` y `[v]`
* `,` = la coma indica que un otro parámetro es próximo
* `[v]vectorscope=b=0.7:m=color3:g=green[v]` = asigna la salida `[v]` al filtro del vectorscopio
* `[m][v]overlay=x=W-w:y=H-h` = superpone el vectorscopio encima de la imagen de vídeo en una cierta ubicación
* `"` = termina el *filter-graph*

{% include figure.html filename="vectorscope.png" caption="Un marco de video de muestra con un vectorscopio" %}

### Forma de Onda (Audio)
Para crear una forma de onda de una sola imagen de un solo canal (mono):

`ffmpeg -i entrada_archivo.ext -filter_complex "aformat=channel_layouts=mono,showwavespic=s=600x240" -frames:v 1 salida_archivo.ext`

* `ffmpeg` = comienza el comando
* `-i entrada_archivo.ext` = la ruta y el nombre del archivo de entrada
* `-filter_complex` = crea un *filter-graph* complejo
* `"` = una comilla comienza el *filter-graph.* La información entre las comillas
 especifica los parámetros de la apariencia y posición de la forma de onda
* `aformat=channel_layouts=mono` = establece la forma de onda para representar la información audio como un canal solo (mono)
* `showwavespic=s=600x240` = activa el filtro `showwavespic`; `s=600x240` designa el tamaño de la imagen como 600 píxeles de ancho y 240 píxeles de alto
* `"` = termina el *filter-graph*
* `-frames:v 1` = asigna la salida a una sola imagen
* `-salida_archivo.ext` = la ruta y el nombre del archivo de salida. La extensión debe ser un formato apropiado de imagen como `.jpeg` o `.png`

{% include figure.html filename="waveform-image.png" caption="Salida de imagen de forma de onda de esto comando" %}

#### Para crear un vídeo de la forma de onda
Este comando generará un archivo de vídeo que muestra la forma de onda de audio en tiempo real:

`ffmpeg -i entrada_archivo.ext -filter_complex "[0:a]showwaves=s=1280x720:mode=line,format=yuv420p[v]" -map "[v]" -map 0:a -c:v libx264 -c:a copy salida_archivo.ext`

* `ffmpeg` = comienza el comando
* `-i entrada_archivo.ext` = la ruta y el nombre del archivo de entrada
* `-filter_complex` = crea un *filter-graph* complejo
`"` = una comilla comienzo el *filter-graph.* La información entre las comillas
 especifica los parámetros de la apariencia y posición de la forma de onda
* `[0:a]showwaves=s=1280x720` = activa el filtro `showwaves` y lo asigna a un cierto tamaño. Este será el tamaño del vídeo de salida.
* `:mode=line,format=yuv420p[v]` = especifica el estilo del gráfico y el espacio de color, `yuv`, y la resolución, `420p`
* `"` = termina el *filter-graph*
* `-map "[v]" -map 0:a` = mapear la salida del *filter-graph* en el archivo de salida
* `-c:v libx264 -c:a copy` = codifica el vídeo del salida con el códec H.264; codifica audio de salida con el mismo códec que el archivo de entrada
* `-salida_archivo.ext` = la ruta y el nombre del archivo de salida

{% include figure.html filename="waveform-video.gif" caption="Representación GIF de la salida de video de esto comando" %}

# Más Recursos
FFmpeg tiene una comunidad grande y bien apoyada de usarios a través de todo el mundo. Como tal, hay muchos recursos gratuitos y de código abierto para descubir nuevos comandos y técnicas para trabajar con materiales audiovisuales. Por favor contacte al autor con cualquier adición a esta lista, especialmente recursos educativos en español para aprender FFmpeg.

* [La Documentación Oficial de FFmpeg](https://www.ffmpeg.org/ffmpeg.html)
* [FFmpeg Wiki](https://trac.ffmpeg.org/wiki/WikiStart)
* [ffmprovisr de Association of Moving Image Archists](https://amiaopensource.github.io/ffmprovisr/)
* [Entrenamiento de Preservación Audiovisual de Ashley Blewer](https://training.ashleyblewer.com/)
* [La Presentación de Andrew Weaver - Demystifying FFmpeg](https://github.com/privatezero/NDSR/blob/master/Demystifying_FFmpeg_Slides.pdf)
* [FFmpeg Presentación de Ben Turkus](https://docs.google.com/presentation/d/1NuusF948E6-gNTN04Lj0YHcVV9-30PTvkh_7mqyPPv4/present?ueb=true&slide=id.g2974defaca_0_231)
* [FFmpeg Cookbook for Archivists de Reto Kromer](https://avpres.net/FFmpeg/)

## Programas de Código Abierto de AV Analysis que usan FFmpeg

* [MediaInfo](https://mediaarea.net/en/MediaInfo)
* [QC Tools](https://bavc.org/preserve-media/preservation-tools)

# Referencias

* Champion, E. (2017) “Digital Humanities is text heavy, visualization light, and simulation poor,” Digital Scholarship in the Humanities 32(S1), i25-i32

* Hockey, S. (2004) “The History of Humanities Computing,” A Companion to Digital Humanities, ed. Susan Schreibman, Ray Siemens, John Unsworth. Oxford: Blackwell
