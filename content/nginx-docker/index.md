+++
title = "Publicar sitio web con Nginx y Docker"
date = 2023-05-07
draft = false 
slug = "nginx"
[taxonomies]
tags = ["nginx", "docker", "vps", "debian"]
+++

## Ingresar al servidor

Dentro de una terminal ingresa el siguiente comando:

```bash 
ssh root@misitio.xyz
```

Esto intentará acceder a tu servidor pidiendo una contraseña para el
usuario root. Puedes reemplazar `misitio.xyz` con el nombre de tu dominio
previamente adquirido y [configurado](@/dns/index.md) o con la dirección
IP del servidor.

## Instalar Docker

Puedes instalar el motor de Docker para un servidor Debian a partir de el
repositorio como se indica en la 
[documentación oficial](https://docs.docker.com/engine/install/debian/#install-using-the-repository).

Si no quieres usar el prefijo *sudo* para ejecutar comandos docker, necesitas
agregar tu usuario al grupo `docker` como se muestra en la 
[documentación](https://docs.docker.com/engine/install/linux-postinstall/).

## Contenedor Nginx proxy

Crear un contenedor con esta 
[imagen](https://hub.docker.com/r/nginxproxy/nginx-proxy) de Docker generará
la configuración de Nginx adecuada cuando se inician o detienen 
contenedores. Acompañado de esta 
[imagen](https://hub.docker.com/r/nginxproxy/acme-companion), generará y
renovará los certificados SSL.

Puedes considerar el siguiente [ejemplo](https://github.com/lu1s9/vigilant-docker/tree/main/nginx-proxy-acme)
,cambiando el nombre de `.env.example` a `.env`, añadiendo tu correo a la 
variable `DEFAULT_EMAIL` de dicho archivo y crear los contenedores mencionados 
con `docker compose up -d`.

A este correo llegarán avisos de certificados que estan próximos a caducar.

## Contenedor de página web

De la misma manera, cambia el nombre del archivo `.env.example` a `.env`,
añade el dominio o subdominio correspondiente a las variables `VIRTUAL_HOST` y 
`LETSENCRYPT_HOST` del siguiente 
[ejemplo](https://github.com/lu1s9/vigilant-docker/tree/main/website).

Finalmente crea el contenedor con `docker compose up -d`.

Ahora puedes actualizar el contenido dentro del directorio `src` y el 
cambio se verá reflejado cuando ingreses tu dominio en el navegador web.

