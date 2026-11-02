---
title: "Referencia de Nmap: comandos esenciales"
description: "Referencia rápida de los comandos más usados de Nmap, con qué hace cada flag y cuándo usarlo."
pubDate: 2026-11-01
category: "herramientas"
draft: false
---

Referencia para usar **solo en redes y sistemas propios o con autorización**.

## Descubrimiento de hosts

```bash
nmap -sn 10.0.0.0/24     # lista hosts activos, sin escanear puertos
nmap -Pn 10.0.0.5        # omite el ping inicial (útil si el host bloquea ICMP)
```

## Escaneo de puertos

```bash
nmap 10.0.0.5                  # los 1000 puertos más comunes
nmap -p 22,80,443 10.0.0.5     # puertos específicos
nmap -p- 10.0.0.5              # todos los puertos TCP (65 535)
nmap -sU --top-ports 20 10.0.0.5   # los 20 puertos UDP más comunes
```

## Servicios y versiones

```bash
nmap -sV 10.0.0.5        # detecta versiones de los servicios
nmap -sC 10.0.0.5        # ejecuta scripts por defecto
nmap -A 10.0.0.5         # detección de SO, versiones, scripts y traceroute (ruidoso)
```

## Guardar la salida

```bash
nmap -oN resultado.txt 10.0.0.5    # texto legible
nmap -oX resultado.xml 10.0.0.5    # XML, para procesar con otras herramientas
nmap -oA resultado 10.0.0.5        # genera los tres formatos a la vez
```

## Buenas prácticas

- Verificá siempre que tenés autorización antes de escanear.
- Empezá con escaneos simples y pasá a los más ruidosos solo si los necesitás.
- Guardá la salida: sin registro, el análisis no se puede reproducir.
