---
layout: post
comments: true
title:  "CRUD simple [ JAVA + SQLITE + MVC ]"
date:   2015-12-15 1:18:21
description: Pequeño proyecto que realiza todas las funciones de un CRUD utilizando el patrón de diseño arquitectónico MVC
categories:
- Java
- POO
- Databases
- Programming
---
Hace ya mucho tiempo que no eh realizado proyectos con java.
Hoy les presento este pequeño proyecto que realiza todas las funciones de un CRUD utilizando el patrón de diseño arquitectónico MVC(Modelo,Vista,Controlador).

![My helpful screenshot]({{ site.url }}/assets/img/java_crud/ventana.png)
<!--more-->

Como base de datos, usamos SQLITE con el driver de xerial.
El proyecto fue creado usando 2 IDE's:

+ **Netbeans** para el front o el diseño de las vistas.
+ **Intellj Idea** para el backend o codificación del mismo.

En el repositorio se incluye la configuración de los IDE's, de tal manera que puedan abrir el proyecto y ejecutarlo sin problemas.

En el algoritmo de arranque del Sistema, si es la primer ejecución o si la versión de la configuración es diferente a la del programa, va a mostrar la siguiente ventana:

![My helpful screenshot]({{ site.url }}/assets/img/java_crud/boot.png)

Al iniciar va a crear una carpeta llamada **crud** en la raíz del directorio de tu usuario, la cual contiene 2 archivos:

+ **_database.sqlite_** - Es nuestra base de datos.
+ **_version_** - Aquí se escribe la versión del programa. Un simple número.

En futuras versiones implementaremos la importación de una configuración anterior, por eso tenemos la opción de importar.

Como estamos hablando de un repositorio de Github, son libres de crear sus propias versiones, pueden descargarlo y si desean pueden reportar incidencias.

- **Repositorio en github:** [Java Crud][repo]
- **Descargar código fuente:** [Java Crud.zip][zip]

Espero les sirva mucho, no olviden dejar un comentario y no dejen de recomendar el blog.

[repo]: https://github.com/shinigamicorei7/java_crud/tree/blogger
[zip]: https://github.com/shinigamicorei7/java_crud/archive/blogger.zip