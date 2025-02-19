# Comandos básicos de Switch con IOS

Al conectarse a algún switch puede aparecer una pantalla en el que se muestra lo siguiente

```
Switch>
```

Ese apartado muestra el nombre predeterminado del Switch (Switch) y el símbolo "`>`". Este símbolo indica que se encuentra en el modo EXEC de usuario.

En dicho modo se tiene acceso a un número limitado de comandos. Para ver los comandos disponibles se puede colocar el signo de interrogación cerrado "`?`" y con esto saldrá un listado con todos los comandos disponibles

```
Switch> ?
Exec commands:
  connect     Open a terminal connection
  disable     Turn off privileged commands
  disconnect  Disconnect an existing network connection
  enable      Turn on privileged commands
  exit        Exit from the EXEC
  logout      Exit from the EXEC
  ping        Send echo messages
  resume      Resume an active network connection
  show        Show running system information
  ssh         Open a secure shell client connection
  telnet      Open a telnet connection
  terminal    Set terminal line parameters
  traceroute  Trace route to destination
```

Entre estos comandos, por ejemplo, aparece el comando "`ping`" que se vió en la clase anterior (clase 2).

El comando "`enable`" es el comando que permite subir de nivel y entrar a modo EXEC privilegiado, el cual permitirá indagar más en la configuración del dispositivo.

Al utilizar el comando "`enable`" se puede visualizar que el simbolo ">" cambió a "#"; esto indica que estamos en modo EXEC privilegiado. Para salir de este modo y volver al modo EXEC de usuario se usa el comando "`exit`".

```
Switch> enable
Switch#
```

Dentro del modo EXEC privilegiado también hay una larga lista de comandos disponibles para su uso. En este modo se tienen 2 comandos importantes, "`show running-config`", el cual muestra la configuración del dispositivo y "`copy running-config startup-config`" que guarda la configuración del mismo. También se puede usar el comando "`write`" para guardar la configuración del dispositivo.

Pero en este modo no se puede realizar la configuración del dispositivo, por lo que para realizar esto se necesita usar el comando "`configure terminal`". Esto pondrá al dispositivo en el modo de configuración global.

```
Switch# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#
```

En este modo se podrá configurar completamente el dispositivo. Para salir del modo podemos usar "`exit`", "`end`" o el atajo del teclado "`ctrl+Z`".

## Comentarios

En este sistema operativo se pueden colocar líneas con comentarios que pueden ayudar a facilitar la lectura de los comandos utilizados. Para esto se debe de colocar el símbolo "`!`" seguido de lo que se desee agregar, por ejemplo:

```
Switch(config)# ! Este es un comentario
```

## Colocar contraseña de acceso al modo EXEC Privilegiado

Para colocar una contraseña al querer ingresar al modo EXEC Privilegiado podemos usar dos comandos:

### Forma 1

```
Switch(config)# enable password miClaveSegura
```

Con este comando se almacena la contraseña `miClaveSegura` en el dispositivo y se debe de ingresar para poder entrar al modo EXEC Privilegiado.

La desventaja de utilizar este comando es que no utiliza un cifrado por lo que al entrar a visualizar la configuración del dispositivo con el comando "`show running-config`" se mostrará la contraseña de la siguiente forma:

```
enable password miClaveSegura
```

Para evitar esto, se puede colocar el comando "`service password-encryption`" en el modo de configuración global. Este comando aplica una cifra reversible (tipo 7) que se encuentran en la configuración del dispositivo.
```
Switch(config)# service password-encryption
Switch(config)# enable password miClaveSegura
```

De esta forma, al correr el comando "`show running-config`" se mostrará la contraseña con algo parecido a la siguiente forma:

```
enable password 7 060506324F41
```

Nota: El número es el resultado de la encriptación tipo 7 de Cisco, que puede ser revertida con herramientas especializadas.

### Forma 2

```
Switch(config)# enable secret miClaveSegura
```

Esto almacena la contraseña con un hash MD5, lo que la hace mucho más difícil de revertir.

Con este comando se almacena la contraseña `miClaveSegura` en el dispositivo utilizando un hash MD5, lo que la hace mucho más dificil de revertir.

## Colocar contraseña de acceso al acceso a la consola y sesiones remotas (Telnet/SSH)

### Contraseña por cable de consola

Para colocar una contraseña desde la conexión física del dispositivo por medio de un cable de consola se puede usar el siguiente comando:

```
Switch(config)# line console 0
Switch(config-line)# password miClave
Switch(config-line)# login
Switch(config-line)# exit
```

Esto obliga a ingresar la contraseña `miClave` (como en el ejemplo) al conectar físicamente con el dispositivo.

### Contraseña por sesión remota

Para colocar una contraseña desde la conexión remota (Telnet/SSH) del dispositivo por medio de un cable de consola se puede usar el siguiente comando:

```
Switch(config)# line vty 0 15
Switch(config-line)# password miClave
Switch(config-line)# login
Switch(config-line)# exit
```

Esto obliga a ingresar la contraseña `miClave` (como en el ejemplo) al conectar de forma remota con el dispositivo.

## Cambio de nombre

Para poderle cambiar el nombre al dispositivo se puede usar el comando "`hostname`", por ejemplo:

```
Switch(config)# hostname SW1
SW1(config)# 
```

El resultado muestra ya que el nombre predeterminado (Switch) se cambió a SW1.

## Cambio de mensaje en banner inicial

Se puede colocar un mensaje en el banner de login, que aparecerá al iniciar el dispositivo. Esto se realiza con el comando "`banner motd $MENSAJE_A_MOSTRAR$`" dentro del modo de configuración global. Por ejemplo:

```
SW1(config)# banner motd $Solo Usuarios Autorizados$
```

Por lo que antes de entrar al modo EXEC de Usuario aparecerá lo siguiente:

```
Solo Usuarios Autorizados

SW1>
```

## Revertir una configuración

Se puede revertir alguna configuración que ya no se desea. Esto se logra simplemente colocando el comando "`no`" seguido por el comando que se ingresó. 

Por ejemplo, si se desea revertir el mensaje de banner inicial, basta con colocar el comando

```
SW1(config)# no banner motd
```

Con este comando se revierte la configuración del banner de login, por lo que ya no aparecerá el mensaje configurado.

Otro ejemplo es también queriendo

## Guardar configuración del dispositivo

Dentro del modo EXEC privilegiado se tiene el comando "`copy running-config startup-config`" que guarda la configuración del mismo. También se puede usar el comando "`write`" para guardar la configuración del dispositivo.

Un ejemplo sería el siguiente:

```
SW1# write
```

Dentro del modo de configuración global se puede colocar la palabra "`do`" antes del comando "`write`" para que pueda guardar la configuración en este nivel. Por ejemplo:

```
SW1(config)# do write
```
