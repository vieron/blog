---
layout: post
title: "Columnas Justificadas en CSS"
date: 2011-09-28 00:16
comments: true
categories: 
  - CSS
---

Hace unos meses, se me metió entre ceja y ceja que tenía que existir alguna manera cross-browser de conseguir columnas justificadas en CSS sin tener que dar anchos ni márgenes. La idea era tratar a las columnas como elementos inline (inline-block, en este caso) y aplicar un <code>text-align:justify</code> al contenedor.

## Primer problema

El text-align: justify; justifica todo excepto la última línea (fila, en este caso). En la práctica estos son los dos casos que no se justifican:

1. Si tienes una sola fila. 
  
  {% jsfiddle R27GS result,css,html default 150px %}
  
2. La última fila, si tienes múltiples filas.

  {% jsfiddle Nmz6N result,css,html default 220px %}
  
  
Empiezo a buscar y me entero que esta técnica se conoce como ["Ben Justification"](http://www.evolutioncomputing.co.uk/technical-1001.html). Encuentro diferentes 
artículos para solucionarlo (aplicado a texto), pero lo hacen añadiendo marcado extra (<code>&lt;span&gt;&amp;nbsp;&lt;/span&gt;</code>) al final del bloque de texto, lo cual no me parece una opción.

En la [w3c](http://www.w3.org/) ya se puede ver en el draft del módulo de texto de CSS3, la propiedad [text-align-last](http://www.w3.org/TR/2011/WD-css3-text-20110901/#text-align-last). Admite los valores <code>auto | start | end | left | right | center | justify </code> donde <code>justify</code> sería el que nos interesaría, pero por desgracia, a día de hoy esto no esta implementado en los navegadores.

Para Internet Explorer, de la versión 5 en adelante, tenemos la propiedad <code markdown="1">[text-justify](http://msdn.microsoft.com/en-us/library/ms531172%28v=vs.85%29.aspx)</code> con el valor <code>distribute-all-lines</code> que nos permite hacer justo lo que queremos.
Del 8 en adelante, la propiedad se usa como una extensión de CSS (con prefijo -ms-), me imagino que será porque esta propiedad aparece ya en las [specs de CSS3](http://www.w3.org/TR/css3-text/#text-justify).

En principio podríamos usarla así:<br />

{% codeblock lang:css %}
-ms-text-justify: distribute-all-lines; /* IE8+ */
text-justify: distribute-all-lines; /* IE5+ */
{% endcodeblock %}

Por tanto tenemos cubierto todo el rango de versiones de Internet Explorer usadas actualmente, lo cual mola, pero nos quedan el resto (Chrome, Safari, Firefox...). Lo primero que pensé después de ver los artículos sobre "Ben Justification" fue que en los navegadores modernos ese marcado extra que se añadía, podría reemplazarse usando el pseudo-elemento [:after](http://www.w3.org/TR/CSS2/generate.html#before-after-content).

{% codeblock lang:css %}
  .grid:after { 
    content: '.'; 
    font-size: 1px; 
    word-spacing: 100%; 
    display: -moz-inline-stack;
    display: inline-block;
    width: 100%; 
    visibility: hidden; 
  }
{% endcodeblock %}

Conseguiríamos tener algo como esto:

{% jsfiddle S987X result,css,html default 220px %}<br /><br />


## Mas "problemitas"

1. Si hay desigualdad entre el número de elementos que entran en cada fila, el comportamiento no suele ser el necesitado.

{% jsfiddle zVBYz result,css,html default 220px %}

2. El elemento generado con el <code>:after</code> hace que haya un espacio después de la última fila, que no he conseguido eliminar. Esto muchas veces no tiene porque ser un problema, pero si tienes un fondo de color como en el ejemplo, quizás si lo sea.

## Código final en limpio

Como a partir de Internet Explorer 8 ya se soportan los pseudo-elementos <code>:after</code> y <code>:before</code>, he evitado usar el <code>-ms-text-justify</code>, para que no se apliquen ambas soluciones.

{% codeblock lang:css %}
.grid {
  text-align:justify;
  text-justify: distribute-all-lines;
 }

.grid:after {
  content: '.';
  font-size: 1px;
  word-spacing: 100%;
  display: -moz-inline-stack;
  display: inline-block;
  width: 100%;
  visibility: hidden;
}

.col {
  display: -moz-inline-stack;
  display: inline-block;    
  *display:inline;
  vertical-align:top;
  zoom:1;
  text-align:left;
}
{% endcodeblock %}

### Para terminar

- Es un patrón bastante común, y se puede usar para estructurar a todos los niveles.
- La gran ventaja de este patrón es su flexibilidad, no tener que establecer anchos ni márgenes de separación entre elementos, es una gran win.
- De momento esta es la mejor solución que he encontrado, pero seguro que se puede mejorar.


#### Referencias:

- [http://www.evolutioncomputing.co.uk/technical-1001.html](http://www.evolutioncomputing.co.uk/technical-1001.html)
* [http://stackoverflow.com/questions/4771304/justify-the-last-line-of-a-div](http://stackoverflow.com/questions/4771304/justify-the-last-line-of-a-div)
- [http://www.cs.tut.fi/~jkorpela/www/justify.html](http://www.cs.tut.fi/~jkorpela/www/justify.html)
- [http://msdn.microsoft.com/en-us/library/ms534671.aspx](http://msdn.microsoft.com/en-us/library/ms534671.aspx)
- [http://archivist.incutio.com/viewlist/css-discuss/30191](http://archivist.incutio.com/viewlist/css-discuss/30191)







