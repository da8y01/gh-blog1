---
layout: post
permalink: /return-jekyllrb-nightskin-font-analytics.html
date: 2022-08-12 12:34:42
tags: JekyllRb jekyll skin night NightSkin font analytics blog
title: "Return to JekyllRb, night skin, font and analytics"
image: https://da8y01.github.io/gh-blog/assets/JekyllrbBlog_NewFont.png
description: "Return to JekyllRb, night skin, font and analytics"
---


El restablecimiento de la configuración original de sitio estático generado tipo blog con [https://jekyllrb.com/][1]{: target="_blank"} debe hacerse y probarse desde sistema operativo Linux (ó Mac, ó WSL) para poder correr e instalar aceptablemente los comandos y paquetes de software necesarios.
* Restaurar archivos y configuración relevante (.gitignore, Gemfile, index).
* Actualizar archivos relevantes de layouts, includes y skins. [https://github.com/jekyll/minima][2]{: target="_blank"}
* Ensayar un nuevo skin (modificando el archivo config):
![JekyllrbBlog_NightSkin][JekyllrbBlog_NightSkin]
* Cambiar la fuente del sitio (descargando los archivos .ttf a los assets locales y definiendo las font-face en los archivos de estilos personalizados disponibles):
![JekyllrbBlog_NewFont][JekyllrbBlog_NewFont]
* Mostrar aspecto básico de las Analytics actuales observadas ([https://analytics.google.com/][3]{: target="_blank"}):
![JekyllrbBlog_CurrentBasicAnalytics][JekyllrbBlog_CurrentBasicAnalytics]
* Poner contador o analíticas personalizadas iniciales.
* Ensayar modo o scripts para skins y contador.
* Ensayar intercambio de internacionalización.


## REFERENCIAS
* [https://jekyllrb.com/][1]{: target="_blank"}
* [https://github.com/jekyll/minima][2]{: target="_blank"}
* [https://analytics.google.com/][3]{: target="_blank"}


[1]: https://jekyllrb.com/
[2]: https://github.com/jekyll/minima
[3]: https://analytics.google.com/


[JekyllrbBlog_NightSkin]: {{ site.baseurl }}/assets/JekyllrbBlog_NightSkin.png "JekyllRb Blog Try Skin"
[JekyllrbBlog_NewFont]: {{ site.baseurl }}/assets/JekyllrbBlog_NewFont.png "JekyllRb Blog New Font"
[JekyllrbBlog_CurrentBasicAnalytics]: {{ site.baseurl }}/assets/JekyllrbBlog_CurrentBasicAnalytics.png "JekyllRb Blog Current Basic Analytics"
