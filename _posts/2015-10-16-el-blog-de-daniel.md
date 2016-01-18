---
layout: post
comments: true
title:  "El Blog de 'Daniel'"
date:   2015-10-16 16:08:45
description: Pequeña introducción a mi blog
categories:
- blog
---
Siempre quise encontrar un grupo de geek's fanáticos a GNU/Linux y a la programación que apoyaran toda idea loca que viene a mi mente, pero lamentablemente fracasé, parece que todas las personas que me rodean tienen cosas mas importantes que hacer.

Desde hace mas de 3 o 4 años e publicado acerca de todo lo que eh aprendido en mi blog [MatrixDevelopments][blog].

Pero me es bastante necesario buscar algo nuevo, y como lo ven aquí estamos.
Probando [Github Pages][github.pages] usando [Jekyll][jekyll] para administrar las publicaciones, para el desing de la página uso el tema [Harmony][template.source] de [gayanvirajith][template.author].

Esto de publicar, es algo complicado por el momento, ya que e _madurado_, estoy casado y el trabajo no me da tiempo. Pero es algo bastante emocionante empezar con algo nuevo.

Y para estrenar este blog, les comparto el famoso **HOLA MUNDO** en los 2 lenguajes de programación favoritos de su servidor.

### PHP
{% highlight php %}
<?php

$hola = function($subject)
{
    return sprintf('Hola %s',$subject);
};

echo $hola('Mundo');

{% endhighlight %}

### Java
{% highlight java %}

public class HolaMundo{
	
    public static void main(String args[]){

        System.out.println("Hola Mundo");

    }
}

{% endhighlight %}

Jaja pronto publicare algo mas interesante.

[blog]: https://matrixdevelopments.blogspot.com
[github.pages]: https://pages.github.com
[jekyll]: http://www.jekyllrb.com
[template.source]: https://github.com/gayanvirajith/harmony
[template.author]: https://github.com/gayanvirajith
