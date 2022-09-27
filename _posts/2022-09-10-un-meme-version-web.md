---
layout: post
permalink: /un-meme-version-web.html
date: 2022-09-10 16:48:42
tags: meme web MemeVersionWeb MemeWebVersion "Meme Versión Web" "Meme Web Version"
title: "Un meme (versión Web)"
image: https://da8y01.github.io/gh-blog/assets/MemeGatoAprende_WebNoEffects.png
description: "Un meme (versión Web)"
---


Se presentan tres (3) versiones de un meme, o la evolución de un meme en tres (3) fases hasta un estado satisfactorio (en cuanto a resultado y proceso).

En este caso resultó más conveniente una versión Web, al menos para satisfacer las expectativas pretendidas, en cuando a resultado final, y lo que implica el proceso.


## [I. Meme Generator](https://letmegooglethat.com/?q=meme+generator+online){: target="_blank"}
En internet hay miles de sitios con funcionalidades o servicios para "generar memes".

Hay de diferente tipo y/o con diferentes opciones, con imágenes famosas predefinidas o personalizadas, con textos y estilos predefinidos o con algunas personalizaciones.

Esta versión que se muestra fue generada en un sitio de los mencionados, subiendo la foto deseada y escribiendo el texto que se visualizaría, tratando de dar algo de estilo, temática o contexto con las opciones disponibles.

<div style="text-align:center" markdown="1">
![MemeGatoAprende_Generators][MemeGatoAprende_Generators]{: width="80%"}
</div>


## [II. Editores de imagen y texto](https://letmegooglethat.com/?q=free+image+editor){: target="_blank"}
Hay miles de programas/software tanto en internet/on-line como instalables, de escritorio o móviles que pueden ser usados como editores de imagen y texto.

Esta versión del meme fue realizada con uno de esos editores, que dió resultados más cercanos a las expectativas, en cuanto a colores, estilos y posicionamiento de los textos y fuentes tipográficas usadas; esto con el enfoque de capturar la inspiración, trasmitir la idea/impresión/información deseada o necesaria, y ser coherente con el conexto del meme en sí.

En los textos se usaron dos (2) fuentes tipográficas, tres (3) colores, y mejor posicionamiento que con la versión de generador.

<div style="text-align:center" markdown="1">
![MemeGatoAprende_Editors][MemeGatoAprende_Editors]{: width="80%"}
</div>


## III. Versión Web
Teniendo la intención de crear un meme, y después de haber ensayado y generado u obtenido unas versiones iniciales del meme aceptables, faltaban detalles para alcanzar a satisfacer las expectativas intuidas/pensadas, como los cinco (5) listados a continuación:

* Tipos de fuentes tipográficas acordes o que aludan al sentido o significado del texto. Por ejemplo una caligrafía estilo script para el texto que dice y alude a "Black Speech", y algo tosco o brusco para el texto que dice y alude a "Mordor".
* Colores, estilización y efectos de los textos y fuentes tipográficas, que transmitan la impresión o idea referente al contexto del meme y esos textos. Por ejemplo un manejo de colores y escala de colores en rojo y naranja para los textos del medio y abajo, y tonalidad azul para el texto de arriba. También referente a esto mismo, efectos (de animación) tipo fuego para el texto referente a "Mordor", la típica tierra volcánica y árida de la temática tratada por el meme.
* Manejo de tonos/matices y escalas de los colores en general del ambiente o entorno del meme (no sólo de los textos), en este caso se usan los tres (3) colores primarios: amarillo, azul y rojo.
* Posicionamiento adecuado de los textos (aunque la opción de Editores también ofrece buenas funcionalidades para este requerimiento).
* No tener que lidiar con transparencias y formatos de imágenes, ya que al no disponer de una fuente tipográfica para cierta versión o formato del meme, y necesitar situarla o usarla en otro sitio, había que manejar el recorte y transparencia de las imáganes con los textos estilizados (aunque sin "efectos", al ser imagánes estáticas) al pasarlos de un procesamiento a otro, con el objetivo de lograr más cercanía con la idea general del meme en elaboración.

Entonces construir esta versión (Web) del meme implicaba los siguientes pasos.
1. Estructura y/o contenido HTML básico.
2. Configuración de las tres (3) fuentes tipográficas utilizadas.
3. Posicionamiento de párrafos de texto.
4. Agregar efectos y animación a las fuentes tipográficas de los textos, y otros retoques.

El contenido y organización, es decir el *layout* o *disposición*, o en general la estructura básica que va a llevar la página web (en este caso el meme) se escribió con las etiquetas o tags esenciales del estándar [HTML][6]{: target="_blank"}.

Se usan tres (3) fuentes tipográficas, cada una configurada de diferente manera o con diferente método, esto con fines educativos.

* La primera fuente tipográfica es la denominada "[Audiowide][3]{: target="_blank"}". Es un (1) archivo de extensión `.ttf` descargado y guardado en un directorio local de la misma página web. Se usa en el primer texto, del tope/arriba que dice "Cuando tu gato Aprende".
* La segunda fuente tipográfica es la denominada "[Vujahday+Script][4]{: target="_blank"}". Es obtenida/traída remotamente o desde internet usando la etiqueta/tag o elemento HTML "[link][7]{: target="_blank"}", *The External Resource Link element*, en la sección "[head][8]{: target="_blank"}", *The Document Metadata (Header) element*, del archivo `.html`. Se usa en el segundo texto, de la mitad, que dice "Black Speech".
* La tercera fuente tipográfica es la denominada "[Rubik+Dirt][5]{: target="_blank"}". Es la *CSS at-rule* "[@import][9]{: target="_blank"}" que invoca/llama la API (Application Programming Interface) remota u on-line de [Google Fonts][1]{: target="_blank"} en el archivo de estilos personalizados `.css` para traer/obtener la fuente y ser utilizada. Se usa en el tercer texto, de abajo/fondo, que dice "de Mordor xD".

<div style="text-align:center" markdown="1">
![MemeGatoAprende_WebNoEffects][MemeGatoAprende_WebNoEffects]{: width="80%"}
</div>


## REFERENCIAS
* [https://fonts.google.com/][1]{: target="_blank"}
* [https://developers.google.com/fonts/docs/getting_started][2]{: target="_blank"}
* [https://fonts.google.com/specimen/Audiowide][3]{: target="_blank"}
* [https://fonts.google.com/specimen/Vujahday+Script][4]{: target="_blank"}
* [https://fonts.google.com/specimen/Rubik+Dirt][5]{: target="_blank"}
* [https://developer.mozilla.org/en-US/docs/Web/HTML][6]{: target="_blank"}
* [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link][7]{: target="_blank"}
* [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head][8]{: target="_blank"}
* [https://developer.mozilla.org/en-US/docs/Web/CSS/@import][9]{: target="_blank"}



[1]: https://fonts.google.com/
[2]: https://developers.google.com/fonts/docs/getting_started
[3]: https://fonts.google.com/specimen/Audiowide
[4]: https://fonts.google.com/specimen/Vujahday+Script
[5]: https://fonts.google.com/specimen/Rubik+Dirt
[6]: https://developer.mozilla.org/en-US/docs/Web/HTML
[7]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link
[8]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head
[9]: https://developer.mozilla.org/en-US/docs/Web/CSS/@import

[MemeGatoAprende_Generators]: {{ site.baseurl }}/assets/MemeGatoAprende_Generators.jpg "Meme Gato Aprende, Generators"
[MemeGatoAprende_Editors]: {{ site.baseurl }}/assets/MemeGatoAprende_Editors.jpg "Meme Gato Aprende, Editors"
[MemeGatoAprende_WebNoEffects]: {{ site.baseurl }}/assets/MemeGatoAprende_WebNoEffects.png "Meme Gato Aprende, Web No Effects"
