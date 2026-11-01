---
title: "Appointment: SQL Injection en el login"
description: "Máquina de Starting Point: enumeración web con Nmap y Gobuster, análisis de un formulario y SQL Injection para saltear la autenticación."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box (Starting Point) · **SO:** Linux · **Dificultad:** Fácil

## Resumen

Un formulario de login no valida correctamente lo que recibe, y eso permite una inyección SQL. Es un ejemplo clásico de por qué hay que usar consultas parametrizadas.

## 1. Enumeración de servicios

```bash
nmap -sC -sV -Pn <IP>
```

Se identifica el puerto TCP/80 con Apache 2.4.38 sobre Debian.

## 2. Enumeración de directorios

```bash
gobuster dir -u http://<IP>/ -w /usr/share/dirb/wordlists/common.txt
```

- `dir` busca rutas dentro del sitio.
- `-w` indica la lista de palabras a probar.

Gobuster encuentra `/vendor/`, donde están las librerías del sitio. Ahí aparece jQuery 3.2.1.

## 3. Analizar el formulario

```bash
curl -i http://<IP>/
```

Con `curl` se revisa el HTML del formulario: los campos, el método (POST) y el nombre de los parámetros, en este caso `username`.

## 4. Probar la inyección

Se envía una petición POST modificando el campo `username`. Una entrada que altera la consulta SQL del servidor hace que el login se acepte sin credenciales válidas.

Por ejemplo, con un payload clásico:

```
' OR '1'='1
```

La respuesta cambia y muestra un mensaje de éxito en lugar del error habitual.

## Aprendizaje

- La inyección SQL ocurre cuando el texto del usuario se mete directamente dentro de la consulta.
- Prevención: usar consultas parametrizadas (prepared statements) y validar la entrada.
- Ver el HTML del formulario y las respuestas HTTP da mucha información sobre cómo funciona la aplicación.
