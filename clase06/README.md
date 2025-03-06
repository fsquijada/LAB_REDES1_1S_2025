# EtherChannel

También conocido como Agregación de Enlaces o como Port Channel, es una técnica para combinar múltiples enlaces físicos en uno lógico con el objetivo de aumentar el ancho de banda, mejorar la redundancia y equilibrar la carga de tráfico entre los enlaces.

## Estático (Manual)

Se configuran manualmente los enlaces sin ningún protocolo de negociación. Es sencilla, pero no verifica si los enlaces están correctamente conectados en ambos extremos.

Para configurar este modo se deben de utilizar los siguientes comandos:

```bash
# En ambos switches
enable
configure terminal
interface Port-Channel 1 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 1 mode on # Se coloca el mismo número que el creado (en este ejemplo 1)
exit
do write # Guarda la configuracion
```

## LACP (Link Aggregation Control Protocol, IEEE 802.3ad)

Protocolo estándar que permite la negociación automática de enlaces entre dispositivos compatibles. Se utiliza en entornos mixtos (no solo Cisco).

Para configurar este modo se deben de utilizar los siguientes comandos:

```bash
# Switch 1
enable
configure terminal
interface Port-Channel 2 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 2 mode active # Se coloca el mismo número que el creado (en este ejemplo 2)
exit
do write # Guarda la configuracion

# Switch 2
enable
configure terminal
interface Port-Channel 2 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 2 mode [active/passive] # Se coloca el mismo número que el creado (en este ejemplo 2)
exit
do write # Guarda la configuracion
```

## PAGP (Port Aggregation Protocol)

Protocolo propietario de Cisco similar a LACP, diseñado para facilitar la configuración automática de EtherChannel entre dispositivos Cisco.

Para configurar este modo se deben de utilizar los siguientes comandos:

```bash
# Switch 1
enable
configure terminal
interface Port-Channel 3 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 3 mode desirable # Se coloca el mismo número que el creado (en este ejemplo 3)
exit
do write # Guarda la configuracion

# Switch 2
enable
configure terminal
interface Port-Channel 3 # Se crea el portchannel
description [descripcion] # Esta descripcion puede ser opcional
exit
interface range FastEthernet 0/1-2 # En esta parte se colocan las interfaces que se desean agrupar
channel-group 3 mode [auto/desirable] # Se coloca el mismo número que el creado (en este ejemplo 3)
exit
do write # Guarda la configuracion
```
## Verificando la configuración

Para mostrar el tipo de configuración que se utiliza, se puede utilizar el siguiente comando en el modo EXEC Privilegiado:

```bash
show etherchannel summary
```
