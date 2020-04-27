---
layout: post
title: 'Block Element Modifier'
date: 2020-04-26
# categories:
---

Al escribir HTML y CSS se requiere nombrar varios elementos usando clases. Muchas veces, nombrarlos puede ser una tarea un poco tediosa y complicada. Además si no tenemos cuidado con los nombres que usamos, volver a trabajar en un proyecto luego de meses podría tardarnos más si es que no hemos usado un estándar al nombre nuestras clases. Volver a familarizarnos con el proyecto puede resultar algo que tome más tiempo que el disponilbe e incluso puede generar frustración.

Para solucionar esto, existen algunas metodologías que buscan hacer más simple estas decisiones. Una de ellas se llama BEM: Block Element Modifier.

## ¿Qué es BEM?

De su página web:

> Block Element Modifier is a methodology that helps you to create reusable components and code sharing in front-end development

> Block Element Modifier es una metodología que ayuda a crear componentes reusables y compartir código en desarrollo web.

Dicho de otra manera, BEM es una guía de estilo para los nombres de clases en HTML y CSS. Como toda guía de estilos es opcional y es algo que se tiene que acordar al momento de empezar un proyecto dentro del equipo.

El objetivo de BEM es crear clases que sean modulares, reusables, específicas e independientes.

## Convenciones

Como el nombre lo dice, Block Element Modifier agrupa los nombres en tres categorías: bloques, elementos y modificadores.

### Bloque

Un bloque se define como una sección que es significativa por si sola dentro de la estructura de la página.

{% highlight html %}

<div class="block">...</div>
{% endhighlight %}

{% highlight css %}

.block {
  text-align: justify;
}
{% endhighlight %}

### Elemento

Parte de un bloque. Existe porque tiene sentido dentro del bloque.

{% highlight html %}

<div class="block">
  <h1 class="block__element"></h1>
</div>
{% endhighlight %}

{% highlight css %}

.block {
  text-align: justify;
}

.block__element {
  font-size: 18px;
}
{% endhighlight %}

### Modificador

Alteran las propiedades de elementos. Se usan para cambiar apariencia, comportamiento o estado.

{% highlight html %}

<div class="block">
  <h1 class="block__element block__element--blue"></h1>
</div>
{% endhighlight %}

{% highlight css %}

.block {
  text-align: justify;
}

.block__element {
  font-size: 18px;
}

.block__element--blue {
  color: #0000ff;
}
{% endhighlight %}


## Consideraciones

### Evitar selectores anidados

La razón de esto es porque BEM genera suficiente especificidad y por lo tanto no hay razón para escribir CSS de esta manera.

{% highlight css %}

.block .block__element {
}
{% endhighlight %}

La única excepción a esta regla es cuando un modificador de un bloque puede modificar a los elementos del bloque.

Se puede utilizar selectores anidados si son declarados por esta razón.

{% highlight css %}

.block--mod .block__element {
}
{% endhighlight %}

### No usar modificadores globales

Tomemos este ejemplo:

{% highlight html %}

<div class="block hidden"></div>

{% endhighlight %}

{% highlight css %}

.block {
  display: block;
}

.hidden {
  display: hidden;
}
{% endhighlight %}

En este caso, no hay problema. El bloque no se muestra porque el `.hidden` aparece luego del bloque en el CSS. Pero si `.hidden` se declara arriba del bloque entonces el elemento seguiría siendo mostrado.

{% highlight css %}

.hidden {
  display: hidden;
}

.block {
  display: block;
}

{% endhighlight %}

{% highlight html %}

<div class="block hidden">not hidden</div>

{% endhighlight %}

Una manera de evitar esto sería poner todos los modificadores en un archivo aparte o al final del CSS siempre. 

Queremos evitar modificadores globales porque ellos nos quitan control de la propiedades que estamos añadiendo. Algunas de ellas podrían crear conflictos entre si y eso nos obligaría a usar `!important` en ciertos casos.

Escribir HTML con modificadores locales tiene más sentido que con modificadores globales por esta razón.

### No nombrar modificadores como las propiedades que aplican

Este tipo de código debe ser evitado.

{% highlight html %}

<div class="block__element block__element--center block__element--line-height-24px"></div>
{% endhighlight %}

{% highlight css %}

.block__element--center {
  text-align: center;
}

.block__element--line-height-24px {
  line-height: 24px;
}
{% endhighlight %}

Existen mejores maneras de nombrar las propiedades `line-height: 24px` y `text-align: center`. Todo depende del contexto en la que estas propiedades se usen. Es mejor juntar varias propiedades de un modificador en una sola clase. Por otro lado, hay pocos casos en los que una clase tiene solamente una propiedad o va a tenerla en el futuro por lo que no es recomendable usar estos nombres.

Esta es una mejor manera de nombrar la clase y agrupar las propiedades.

{% highlight html %}

<div class="block__element block__element--mod"></div>
{% endhighlight %}

{% highlight css %}

.block__element--mod {
  text-align: center;
  line-height: 24px;
}
{% endhighlight %}

### No nombrar elementos dentro de otros elementos `block__el1__el2`

Es mejor nombrarlos de la siguiente manera.

{% highlight html %}

<div class='block'>
    <div class='block__elem1'>
        <div class='block__elem2'>
            <div class='block__elem3'></div>
        </div>
    </div>
</div>
{% endhighlight %}

## Ventajas y desventajas

### Ventajas

- Facilitan el mantenimiento del código. Hacen que sea fácil distinguir una sección de otra, particularmente en el CSS donde no tenemos una vista de la estructura de la página como en el HTML.
- Crea partes reusables y modulares de manera natural.
- Es una metodología flexible. Si bien las convenciones de BEM son estrictas, se pueden adaptar de otras formas y a cualquier base de código existente.

### Desventajas

- Recarga la hoja de estilos de selectores.
- Puede generar desorden.

## Otras metodologías

Existen otras metodologías que tienen el mismo objetivo que BEM, pero que atacan el problema de maneras diferentes. No voy a hablar mucho de ellos en este artículo porque creo que merecen artículos propios.

- **[OOCSS (Object Oriented CSS)](http://oocss.org/):** Busca separar el contenedor y el contenido con "objetos" en el CSS. La información en la página web es escasa y confusa. El Github de este proyecto parece abandonado ya que su último actividad fue en el 2013. Una mejor explicación sobre OOCSS se encuentra en este [artículo](https://www.keycdn.com/blog/oocss). 
- **[SMACSS (Scalable and Modular Architecture for CSS)](http://smacss.com/):** Es una guía de estilo para escribir CSS con cinco categorías para reglas de CSS. El autor estuvo trabajando por última vez en re-escribir el proyecto el el 2016, pero su aplicación, como la de BEM, sigue siendo vigente ya que se trata de una metodología.
- **[SUITCSS](http://suitcss.github.io/):** Es una metodología confiable y testeable para componentes de UI. Esta metodología incluye herramientas para pre-procesadores de CSS y otras herramientas de desarrollo. La última actividad en su Github fue en el 2017.
- **[Atomic](http://github.com/nemophrost/atomic-css):** Divide los estilos en piezas atómicas e indivisibles. La razón de ser y las reglas de Atomic se pueden encontrar en este [artículo](https://www.smashingmagazine.com/2013/10/challenging-css-best-practices-atomic-approach/).

## Sanity check

Nombrar elementos y funciones en programaciones, de por si ya es complicado. Hay que recordar que estas reglas son opcionales y, sobre todo, son adaptables. Si alguna de estas reglas no se adapta a un proyecto puede ser modificadas según como más sea conveniente.

El objetivo de BEM es que el código sea mantenible y fácil de compartir. Busca ahorrar tiempo al momento de crear elementos en la web.
