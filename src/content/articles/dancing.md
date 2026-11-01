---
title: "Dancing: SMB con acceso anónimo"
description: "Máquina de Starting Point en Windows: enumeración de recursos SMB, acceso anónimo y descarga de archivos con smbclient."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box (Starting Point) · **SO:** Windows · **Dificultad:** Fácil

## Resumen

Un recurso compartido por SMB permite acceso sin contraseña. Con las herramientas adecuadas se navegan las carpetas y se descargan archivos.

## 1. Escaneo de servicios

```bash
nmap -sV <IP>
```

Se identifican los servicios SMB abiertos en la máquina.

## 2. Instalar smbclient (si hace falta)

```bash
sudo apt install smbclient
```

Si el comando no existe, instalalo con APT. Las dependencias se resuelven solas.

## 3. Listar recursos compartidos

```bash
smbclient -L //<IP> -N
```

- `-L` lista los recursos compartidos.
- `-N` no pide contraseña (acceso anónimo).

## 4. Entrar al recurso

```bash
smbclient //<IP>/WorkShares -N
```

## 5. Navegar y descargar

Dentro de smbclient:

```
ls
cd <carpeta>
get <archivo>
```

Recorré las carpetas con `ls` y `cd`. Descargá los archivos que parezcan importantes con `get`.

## 6. Leer la evidencia

Ya fuera de smbclient, en la terminal:

```bash
cat flag.txt
```

## Aprendizaje

- Un recurso compartido sin contraseña es una mala configuración de permisos, no una vulnerabilidad de ejecución remota.
- Enumerar antes de explotar: muchas veces la información valiosa está en archivos que ya son accesibles.
- Hay que revisar qué carpetas se comparten y con quién.
