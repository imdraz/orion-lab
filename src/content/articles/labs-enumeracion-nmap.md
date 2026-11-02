---
title: "Laboratorio: enumeración básica de un host con Nmap"
description: "Práctica guiada en un laboratorio propio: descubrir hosts, puertos y servicios con Nmap, y documentar el proceso."
pubDate: 2026-11-01
category: "laboratorios"
draft: false
---

Esta práctica se realiza en un **entorno de laboratorio propio** (por ejemplo, una máquina virtual con Metasploitable o una red local de VirtualBox). Nunca escanees sistemas sin autorización por escrito.

## Objetivo

Aprender a hacer un reconocimiento básico con Nmap: identificar hosts activos, puertos abiertos y los servicios que corren en ellos.

## Preparación

- Una máquina atacante (Kali Linux o cualquier distro con Nmap instalado).
- Una máquina objetivo dentro de una red virtual aislada.
- Anotar la IP de cada equipo con `ip a`.

## Paso 1: descubrir hosts activos

```bash
nmap -sn 192.168.56.0/24
```

`-sn` hace un escaneo de descubrimiento (ping scan) sin escanear puertos. Sirve para saber qué equipos están encendidos en la red.

## Paso 2: escaneo de puertos

```bash
nmap -sS -p- 192.168.56.101
```

- `-sS`: escaneo SYN, rápido y discreto.
- `-p-`: revisa los 65 535 puertos TCP.

## Paso 3: detección de servicios y versiones

```bash
nmap -sV -sC -oN scan_inicial.txt 192.168.56.101
```

- `-sV`: detecta la versión de cada servicio.
- `-sC`: ejecuta scripts por defecto para obtener más información.
- `-oN`: guarda el resultado en un archivo, útil para documentar.

## Qué documentar

1. Hosts encontrados y su rol esperado.
2. Puertos abiertos y servicios con sus versiones.
3. Posibles riesgos, por ejemplo servicios desactualizados o expuestos sin necesidad.
4. Próximos pasos de investigación (sin explotar nada fuera del laboratorio).

## Conclusión

El reconocimiento es la base de cualquier evaluación. Un buen informe distingue lo que se observó de lo que se supone, y registra cada comando usado para que otra persona pueda repetir el proceso.
