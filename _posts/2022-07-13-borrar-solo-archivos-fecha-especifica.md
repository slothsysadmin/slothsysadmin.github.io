---
layout: post
title: ¿Como borrar solo los archivos creados antes de una fecha especificada?
author: rangel
categories: sysadmin
tags: bash linux scripting find
---

Nos estamos quedando sin espacio en el disco o una partición en concreto y queremos borrar un conjunto de archivos más antiguos a una fecha especificada, por ejemplo los logs que tengan más de un año (si... aún no hemos configurado el logrotate y nos toca trabajar...)

La opción más rápida y fácil para hacerlo seria con el comando `find`:<!--more-->

{% highlight bash %}
find . -type f ! -newrmt '31/06/2021 00:00:00' -delete;
{% endhighlight %}

### **Explicación de la instrucción:**
- **.**: hace referencía al path donde buscar, el '.' indica que buscará en el directorio actual
- **-type f**: especificamos que solo debe mostrar los archivos, omitiendo así los directorios.
- **! -newrmt '31/06/2021 00:00:00'**: indica que debe mostrar los elementos cuya fecha de modificación sea anterior al 31 de junio de 2021.
- **-delete**: es la acción que tomaremos con los resultados obtenidos, para este caso 'borrar'

term
: defi
: defi2

```
Atención! Unix no registra la fecha de creación, por lo que solo disponemos de 3 registros de tiempo en los archivos: último acceso, última modificación del contenido y última modifición del inodo.
```

Así que según el registro de tiempo que queramos utilizar debermos escoger entre:
- **-newermt:** última modificación
- **-newerat:** último acceso
- **-newertct:** último cambio del inodo




Os invito a trastear con el comando `find` pues si lo domináis seguro que os ahorra mucho tiempo, y podéis automatizar algunas de vuestras tareas. Ya no sólo por lo potente que és en cuanto a la búsqueda de archivos sino también para la múltitud de opciones que tiene con el tratamiento de los resultados.

[Documentación oficial del comando `find`](https://linux.die.net/man/1/find "find man page")
