+++
title = "Acceder a un servidor mediante SSH"
date = 2023-05-10
draft = false 
slug = "ssh"
[taxonomies]
tags = ["ssh", "vps", "debian"]
+++

Acceder a un servidor con claves SSH tiene dos principales ventajas:

- **Acceder sin contraseña:** Marcando como dispositivo de confianza a tu
computadora local para acceder al servidor.
- **Aumenta la seguridad:** Dado que ahora no necesitas una contraseña para
acceder, puedes desactivar por completo esta opción, permitiendo solo el
acceso a tus dispositivos previamente indicados.

## Acceder al servidor

Accede desde la terminal a tu servidor con las credenciales apropiadas.

```bash
ssh root@142.250.69.14
```

Puedes cambiar la dirección IP por tu dominio si ya lo 
[configuraste](@/dns/index.md) previamente.

## Crear usuario no root

No es buena práctica acceder como usuario root mediante ssh, por lo cual será
necesario crear un usuario con los privilegios adecuados. Además necesitarás 
tener instalado el paquete `sudo`.

```bash
useradd -m -s /bin/bash -G sudo usuario
passwd usuario
```

- La opción `-m` creará el directorio *home* para el usuario indicado.
- La opción `-s` indica la `shell` a usar.
- La opción `-G` indica los grupos suplementarios del nuevo usuario.
- El comando `passwd` asignará la contraseña al usuario indicado.

Ahora puedes salir de la sesión y volver a entrar con el usuario y contraseña 
previamente indicada,

## Generar clave SSH

Desde tu equipo local puedes generar una clave ssh con el siguiente comando:

```bash
ssh-keygen -t ed25519 -C "tu_email@ejemplo.com"
```

Puedes dejar la ruta del archivo por defecto y asignar una *passphrase* para
agregar una capa de seguridad.

## Copiar clave a tu servidor

Con el siguiente comando, tu computadora añadirá la clave ssh pública a una 
lista de dispositivos autorizados en el servidor, cambiando el usuario y
dirección IP correspondiente.

```bash
ssh-copy-id usuario@142.250.69.14
```

Ahora podrás acceder desde ese dispositivo al servidor sin necesidad de
ingresar la contraseña en cada ocasión.

## Configuración de Seguridad de SSH

Algunos de los ajustes básicos recomendados para mantener la seguridad 
de tu servidor involucran:

- Denegar el acceso como usuario *root*.
- Evitar el acceso con contraseñas.

Cambia y comprueba los siguientes valores dentro del archivo 
`/etc/ssh/sshd_config`.

```bash
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
```

Y reinicia el servicio.

```bash
sudo systemctl reload sshd
```

Adicionalmente, puedes 
[configurar el firewall](@/configurar-firewall-servidor/index.md) para 
aumentar aún más la seguridad de tu servidor.

