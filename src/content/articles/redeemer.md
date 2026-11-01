---
title: "Redeemer: Redis sin autenticación"
description: "Máquina de Starting Point: enumeración de Redis expuesto, consulta de configuración y lectura de claves."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box (Starting Point) · **SO:** Linux · **Dificultad:** Fácil

## Resumen

Redis está expuesto en el puerto 6379 y no tiene contraseña configurada. Cualquiera que llegue al puerto puede consultar sus datos.

## 1. Detección del servicio

```bash
nmap -sV <IP>
```

Se identifica Redis 5.0.7 en el puerto TCP/6379.

## 2. Conexión con redis-cli

```bash
redis-cli -h <IP> -p 6379
```

## 3. Comprobaciones básicas

```
PING
INFO server
INFO clients
```

- `PING` comprueba que el servicio responde.
- `INFO server` muestra datos de la versión y del sistema.
- `INFO clients` muestra las conexiones.

## 4. Revisar la contraseña

```
CONFIG GET requirepass
```

Si el valor está vacío, el servidor no pide autenticación.

## 5. Enumerar las claves

```
SELECT 0
DBSIZE
INFO keyspace
SCAN 0
KEYS *
```

- `SELECT 0` elige la base de datos 0.
- `DBSIZE` cuenta las claves.
- `KEYS *` lista todas las claves (en producción conviene evitarlo, porque bloquea el servidor).

## 6. Leer valores

```
GET <nombre_de_la_clave>
```

En el laboratorio aparecen cuatro claves. Se lee el valor de cada una con `GET` hasta encontrar la de la evidencia.

## Aprendizaje

- Un servicio de datos nunca debe quedar expuesto a internet ni a la red sin autenticación.
- `requirepass` vacío significa que no hay contraseña.
- Los comandos como `KEYS` y `CONFIG` pueden desactivarse en Redis para reducir la superficie de ataque.
