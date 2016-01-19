---
layout: post
comments: true
title:  "Sistema de admisiones"
date:   2016-01-18 14:49:20
categories:
- Php
- POO
- Databases
- Programming
---

Hace 6 meses cree este sistema, que se encuentra en producción, y en los primeros días de uso se presentaron varias incidencias.
Ya que el mismo no tuvo tiempo de testeo, y no hubo tiempo para poder realizar pruebas unitarias.

![página de inicio de sesión]({{ site.url }}/assets/img/sistema-de-admisiones/admisiones.png)

Para este sistema creé un micro-framework nada profesional que busca manejar el modelo arquitectónico **MVC** y que se quedo
en **VC** ya que nunca se implementaron los modelos :smile:.

Como esta escrito en la documentación oficial:

> Es un sistema desarrollado a medida para la gestión del proceso
> de admisión de las Instituciones Educativas de FINDES o COPADE.

Para este proyecto no tenemos disponible **MOD_REWRITE** así que usamos el _query_string_ para el Routing o gestión de rutas

El nivel de php es 5.3.*

Si se preguntan el porque de tantas limitaciones, pues es por que no soy administrador del vps :cold_sweat:.

Si pueden fijarse, este proyecto es de uso especifico o exclusivo, la idea de liberar el código es que podamos usarlo
para otros proyectos, claro que hay que mejorarlo mucho, pero eh ahi el reto.

Mis metas para este proyecto son las siguientes:

## TODO

- [ ] Implementar composer para la auto-carga de clases.
- [ ] Agregar un gestor de rutas, algo mas profesional.
- [ ] Mejorar el ACL <small>(Access Control List)</small>.
- [ ] Hacer opcional cada parte del proceso, que cada institución pueda tener un proceso diferente.
- [ ] Implementar un **password recovery** o una forma de recuperar su clave.

Por el momento esto es lo que tengo en mente, conforme avance con el desarrollo creo que aparecerán mas necesidades.

## Instalación

Para que podamos empezar a usar nuestro proyecto, tenemos que descargar el código fuente y la base de datos:

Importamos la base de datos y en los fuentes del proyecto buscamos el archivo `config/database.php` y actualizamos los parámetros de conexión.

Los usuarios son los siguientes:

| Usuario       | Clave  |        Tipo              |
|:--------------|:------:|:------------------------:|
| 1723471478    | 12345  | Administrado de Ventas   |
| 1723471478001 | 12345  | Secretario Institución 1 |
| 1723471478002 | 12345  | Secretario Institución 2 |
| 1723471478003 | 12345  | Secretario Institución 3 |

### Enlaces

- [Código fuente](https://github.com/shinigamicorei7/sistema-admisiones/archive/master.zip)

- [Base de datos](https://www.dropbox.com/s/4jndvcveyfoh5i3/sitema_de_admisiones.sql?dl=0)

