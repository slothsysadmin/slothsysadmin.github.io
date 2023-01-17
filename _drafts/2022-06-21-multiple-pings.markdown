---
layout: post
author: rangel
title: "Pings automáticos a varias IPs"
date: 2022-06-21 12:10:31 +0200
categories: sysadmin lazy-tips
tags: bash windows sysadmin-tools scripting

---

Es habitual que en nuestro dia a dia utilizemos la herramienta `ping` para saber si una dirección está viva y/o accesible, así como tener una aproximación sobre si esa IP la podemos asignar a un equipo o dispositvio sin problemas.

Què pasa pero cuando queremos averiguar el estado de más de una dirección, ya sea un rango de IPs o de toda una red?

Tenemos que lanzar el comando manualmente para cada una de las direcciones?

> **Esto es un trabajo tedioso y muy aburrido!**
> 
> **No nos aporta ningún valor y encima nos hace perder mucho tiempo!**

<!-- En esos momentos en que tenemos que monitorizar el estado de varias IPs en nuestra red para saber si estan vivas y/o accesibleas así como verificar

Muchas veces tenemos la necesidad de monitorizar las máquinas que hay en nuestra red, ya sea para saber si estan encendidas y/o accesibles o para saber si hemos asignado ya una IP a un equipo antes de asignarla a otro y evitar problemas de duplicidad de IPs.

Lo más habitual en estos casos, es lanzar un ping a la IP y esperar el resultado. El problema está en cuando necesitamos investigar más de una IP y hacerlo manualmente una por una se convierte en una tarea muy tediosa, sobretodo para los buenos "sysadmins perezosos".

Para ello, en este post explico como automatizar estas tareas con un bucle en el própio intérprete de comandos de Windows: -->

Para ello, en este post, os voy a explicar como automatizar esta tarea en una sola línea que podéis lanzar en vuestra línia de comandos de Windows:

{% highlight bash %}
for /L %i in (1,1,255) do ping -n 2 192.168.0.%i
{% endhighlight %}

Se trata de implementar un bucle en el que pediremos que se vaya desde la posición 1 (paràmetro 1) a la 255 (paràmetro 3) en intervalos de a 1 (paràmetro 2).

Para no hacer muy larga la ejecución he especificado el paràmetro `-n 2` así de esta forma solo hace 2 peticiones `echo`, suficiente para determinar el resultado en caso que la IP esté viva.

Luego, solo falta redirigir la salida para guardar los resultados en un fichero en lugar de verlos por pantalla:

{% highlight shell %}
for /L %i in (1,1,255) do ping -n 2 192.168.0.%i > ping.txt
{% endhighlight %}

De esta forma podemos lanzar la ejecución en background o programarla en un cron si nos interesa automatizarla cada cierto tiempo.

Espero que os haya servido este truco y os haga la vida un poco más fácil.

Hasta el próximo post!
