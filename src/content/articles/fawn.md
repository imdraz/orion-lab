---
title: "Fawn: FTP anónimo"
description: "Máquina de Starting Point: reconocimiento de FTP, acceso anónimo y descarga de archivos con el cliente FTP."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box (Starting Point) · **SO:** Unix · **Dificultad:** Fácil

## Resumen

El servidor FTP permite el acceso anónimo. Con ese acceso se listan y descargan archivos sin contraseña personal.

## 1. Comprobar conectividad

```bash
ping -c 3 <IP>
```

## 2. Enumeración de servicios

```bash
nmap -sV -sC -Pn <IP>
```

El puerto TCP/21 está abierto y el servicio es vsftpd 3.0.3. Nmap también confirma que el acceso anónimo está habilitado.

## 3. Instalar el cliente FTP

```bash
apt policy ftp
sudo apt install ftp
```

`apt policy` muestra si el paquete está disponible y en qué versión.

## 4. Conectarse

```bash
ftp <IP>
```

Cuando pida el usuario, escribí `anonymous`. No hace falta contraseña.

## 5. Listar y descargar

Dentro de la sesión FTP:

```
ls
get flag.txt
quit
```

- `ls` lista los archivos del servidor.
- `get` descarga el archivo a tu máquina.
- `quit` cierra la sesión.

## 6. Leer el archivo

Ya fuera de FTP, en la terminal de Linux:

```bash
cat flag.txt
```

## Aprendizaje

- Los comandos del cliente FTP (`ls`, `get`) no son comandos de Linux. Dentro de FTP se usan los suyos, y después volvés a la terminal.
- Si un archivo no aparece o no se descarga, puede ser un problema local (permisos, carpeta de destino) o del servidor (modo activo o pasivo).
- Un FTP anónimo es una mala configuración: no debería estar habilitado si no hace falta.
