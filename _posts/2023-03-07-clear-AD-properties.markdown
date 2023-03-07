---
layout: post
author: rangel
title: "Vaciando propiedad de usuario en Active Directory"
date: 2023-03-07 12:10:31 +0200
categories: sysadmin lazy-tips automation
tags: windows sysadmin-tools scripting powershell activedirectory
---

En el pasado, al disponer de los servicios de Blackberry para el correo electrónico teníamos la configuración del reenvió smtp en el atributo `proxyAddresses` para cada usuario del Directorio Activo.

Hoy en día, utilizando Zimbra como aplicación de correo electrónico esto no tiene ningún sentido, por lo que vamos a proceder a limpiar dicha propiedad.

<!--more-->

Para ello, vamos a hacer uso del siguiente comando en PowerShell:

{% highlight PowerShell %}
Set-ADUser -Identity "NombreUsuario" -Clear proxyAddresses
{% endhighlight %}

Ya que el problema aplica a más de un usuario, lo óptimo será crear un script que haga la consulta a nuestro directorio para todos aquellos que tengan algo en la variable y proceder luego a vaciarla.

Como no queremos llevarnos ningún susto, vamos a guardar en un archivo csv los datos de la variable para cada usuario, de forma que si hiciese falta podríamos generar un script para restaurar la información que ya había.

El código quedaría de la siguiente forma:

{% highlight PowerShell %}
Import-Module ActiveDirectory

$fechaActual = (get-date)
$fileName = "mailProxy" + (get-date -Format "yyyyMMdd") + ".csv"

#Crea un nuevo archivo donde guardar los datos de los usuarios
Set-Content $fileName "username;proxy"

#Filtrar los usuarios habilitados y con la propiedad proxyAddresses establecida
$usuariosLDAP = get-aduser -filter * -SearchBase "dc=tu-dominio,dc=local" -properties * | where {$_.Enabled -eq $true} | where {$_.proxyAddresses}

foreach ($user in $usuariosLDAP)
{
  $nombreUsuario = $user.sAMAccountName
  $mailProxy = $user.proxyAddresses

  if (!$mailProxy) {
	continue
  }

  $fileMsg = "$nombreUsuario;$mailProxy"

  #Se guardan los datos a modo backup
  Add-Content $fileName $fileMsg

  Set-ADUser -Identity $user -Clear proxyAddresses
}
Write-Output ("Número usuarios afectados: " + $usuariosLDAP.Count)
{% endhighlight %}

Como podéis ver, es muy fácil modificar la propiedad que queremos vaciar por la que más nos convenga.

Espero que os haya servido este script y os haga la vida un poco más fácil.

Hasta el próximo post!
