+++
title = "Seguridad para tu servidor linux"
date = 2023-05-14
draft = false 
slug = "firewall"
[taxonomies]
tags = ["firewall", "debian", "vps"]
+++

Como medida adicional de seguridad para tu servidor puedes prevenir el acceso 
a intrusos e imponer sanciones con una herramienta llamada *Fail2ban*. 
Adicionalmente, puedes configurar el cortafuegos para controlar tráfico 
entrante o saliente con *Uncomplicated Firewall*.

En Debian, el primer paquete se instala con el siguiente comando:

```bash
sudo apt install fail2ban
```

Comprobando que se haya inicializado el servicio.

```bash
systemctl status fail2ban
```

Los valores predeterminados de esta herramienta bloquearán por 10 minutos las 
conexiones que hayan fallado 5 veces al intentar acceder a tu servidor, 
aunque se pueden [modificar](https://wiki.archlinux.org/title/Fail2ban) estos 
valores.

##  Firewall

La manera más sencilla de controlar el tráfico entrante y saliente es mediante
el paquete *ufw*.

Puedes instalar, configurar, habilitar y comprobar el servicio con los 
siguientes comandos con privilegios root.

```bash
apt install ufw
ufw default deny incoming
ufw default allow outgoing
ufw allow ssh
ufw enable
ufw status
```

Se permite tráfico por medio de ssh, de lo contrario no podrás acceder al
servidor.
