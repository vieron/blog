---
layout: post
title: "Columnas Justificadas en CSS"
date: 2011-09-28 00:16
comments: true
categories: 
---

Hace unos meses, se me metio entre ceja y ceja que tenía que haber una manera cross-browser de hacer columnas justificadas sin tener que darles ancho ni margenes. La idea era tratar a las columnas como elementos inline (inline-block, en este caso) y aplicar un text-align:justify; al contenedor.

¿Cúal es el primer problema?

El text-align: justify; justifica todo excepto la última línea (fila, en este caso). En la práctica estos son los dos casos que no se justifican:

- Si tienes una sola fila. 
- La última fila, si tienes multiples filas.

Esto se entiende mejor gráficamente, por ejemplo:

---- IMAGEN ----- CASO UNA LINEA

---- IMAGEN ----- CASO MÚLTIPLES LINEAS





