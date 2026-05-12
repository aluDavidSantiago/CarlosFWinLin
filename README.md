# Integración de Zorin OS en Active Directory

## Descripción

Este repositorio documenta el proceso de integración de un sistema Zorin OS dentro de un dominio Active Directory en Windows Server 2022.

El objetivo es permitir la autenticación de usuarios del dominio en un cliente Linux, junto con la gestión de permisos y el acceso a recursos compartidos en red.

***

## Entorno

*   Windows Server 2022 (Controlador de dominio)
*   Zorin OS (Cliente Linux)
*   Dominio: `foodlogic10.test`

***

## Funcionalidades implementadas

*   Unión del cliente Linux al dominio
*   Autenticación mediante usuarios de Active Directory
*   Creación automática de directorios personales
*   Configuración de privilegios administrativos (sudo)
*   Restricción de acceso a usuarios específicos
*   Acceso a recurso compartido desde Linux
*   Escritura en carpeta compartida mediante CIFS

***

## Pasos principales

### Unión al dominio

```
sudo realm join foodlogic10.test -U Administrador
```

***

### Verificación

```
realm list
```

***

### Login con usuario del dominio

```
su - Administrador@foodlogic10.test
```

***

### Configuración de home automático

```
sudo pam-auth-update
```

Activar:

    Create home directory on login

***

### Configuración de permisos sudo

```
sudo visudo
```

Añadir:

    administrador ALL=(ALL) ALL

***

### Control de acceso

```
sudo realm permit administrador
sudo realm deny --all
```

***

### Montaje de carpeta compartida

```
sudo apt install cifs-utils
sudo mkdir /mnt/compartido

sudo mount -t cifs //10.0.2.6/Compartido /mnt/compartido \
-o user=administrador,uid=1000,gid=1000,file_mode=0777,dir_mode=0777
```

***

### Verificación de acceso

```
ls /mnt/compartido
touch /mnt/compartido/prueba.txt
```

***

## Resultado

El cliente Linux queda integrado en el dominio y es capaz de:

*   autenticarse contra Active Directory
*   aplicar permisos administrativos
*   acceder y escribir en recursos compartidos

***

## Autor

Santiago Hernandez

