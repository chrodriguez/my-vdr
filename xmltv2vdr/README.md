# xmltv2vdr

En casa usamos cablevision analogico y TDA. Este contenedor manipula la gestión de canales para cablevisión analógico y su EPG

Para integrar VDR con los canales del cable y su EPG, debemos:

* *Actualizar la configuración de xmltv:* regenerar su configuracion en `config/tv_grab_ar.conf` cuando el cable cambia la grilla
* *Generar canales para VDR:* genera un channels.conf con los canales de cablevision en `config/vdr-channels.conf`
  * Luego, *manualmente* deberá actualizarse la configuracion del VDR instalado `config/channels.conf`
* En un cron, correr periodicamente la actualizacion de la EPG 

## Como actualizar la configuracion de xmltv

Correr en el directorio actual

```
docker run -it --rm -v `pwd`/bin:/script -v `pwd`/config/:/etc/xmltv chrodriguez/xmltv /script/configure_tv_grab_ar
```

Esto actualizará `config/tv_grab_ar.conf`

## Como generar canales para VDR

Correr en el directorio Raíz del proyecto:

```
docker run --rm -v `pwd`/bin:/script -v `pwd`/config/:/etc/xmltv chrodriguez/xmltv /script/generate-vdr-channels
```

Esto actualizará `config/vdr-channels.conf`

## Cron para actualizar EPG

Editar el crontab del usuario con `crontab -e` y agregar:

```
SHELL=/bin/bash
01 2 * * * cd <PATH_RAIZ_REPO> && \
	/usr/bin/docker run --rm -v `pwd`/bin:/script \
		-v `pwd`/config/:/etc/xmltv \
		-e VDR_SERVER=my-vdr.hostname.example.net \
		-e TZ=America/Argentina/Buenos_Aires \
		chrodriguez/xmltv /script/update-epg-vdr
```

Notar que debe reemplazar la variable *VDR_SERVER* por un nombre o IP donde está instalado VDR
