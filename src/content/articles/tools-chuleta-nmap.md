---
title: "Chuleta de Nmap: comandos esenciales"
description: "Referencia rápida de los comandos más usados de Nmap, con qué hace cada flag y cuándo usarlo."
pubDate: 2026-11-01
category: "tools"
draft: false
---

Chuleta de referencia para usar **solo en redes y sistemas propios o con autorización**.

## Descubrimiento de hosts

| Comando | Qué hace |
|---|---|
| `nmap -sn 10.0.0.0/24` | Lista hosts activos sin escanear puertos. |
| `nmap -Pn 10.0.0.5` | Omite el ping inicial; útil si el host bloquea ICMP. |

## Escaneo de puertos

| Comando | Qué hace |
|---|---|
| `nmap 10.0.0.5` | Escanea los 1000 puertos más comunes. |
| `nmap -p 22,80,443 10.0.0.5` | Escanea puertos específicos. |
| `nmap -p- 10.0.0.5` | Escanea todos los puertos TCP. |
| `nmap -sU --top-ports 20 10.0.0.5` | Escanea los 20 puertos UDP más comunes. |

## Servicios y versiones

| Comando | Qué hace |
|---|---|
| `nmap -sV 10.0.0.5` | Detecta versiones de servicios. |
| `nmap -sC 10.0.0.5` | Ejecuta scripts por defecto. |
| `nmap -A 10.0.0.5` | Combina detección de SO, versiones, scripts y traceroute. Es ruidoso. |

## Salida

| Flag | Formato |
|---|---|
| `-oN archivo.txt` | Texto legible. |
| `-oX archivo.xml` | XML, útil para procesar con otras herramientas. |
| `-oA base` | Genera los tres formatos a la vez. |

## Buenas prácticas

- Verificá siempre que tenés autorización antes de escanear.
- Empezá con escaneos simples y pasá a los más ruidosos solo si los necesitás.
- Guardá la salida: sin registro, el análisis no se puede reproducir.
