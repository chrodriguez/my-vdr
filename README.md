# VDR para La Plata / Argentina

## Instalación

Correr el script `./install` por única vez. Esto creará un contenedor *vdr*

## Inicio automático con upstart

* Crear un link duro o copiar el archivo `upstart/vdr.conf` a `/etc/init`
* Frenar el contenedor: `docker stop vdr`
* Notificar a upstart del nuevo servicio: `initctl reload-configuration`
* Iniciar el servicio: `service vdr start`

## Actualizacion de canales cablevision y EPG

Leer [README de xmltv2vdr](xmltv2vdr/README.md)
