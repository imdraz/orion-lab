---
title: "Meow: acceso por Telnet"
description: "Máquina de Starting Point: enumeración de servicios, conexión a Telnet y verificación de privilegios."
category: "laboratorios"
draft: false
---

**Plataforma:** Hack The Box (Starting Point) · **SO:** Linux · **Dificultad:** Fácil

## Resumen

Meow tiene el servicio Telnet expuesto en el puerto 23. Con el reconocimiento adecuado se llega a una sesión remota sin técnicas complejas.

## 1. Enumeración de servicios

```bash
nmap -sC -sV -T4 -Pn <IP>
```

El resultado muestra el puerto TCP/23 abierto con el servicio Telnet.

## 2. Conexión a Telnet

```bash
telnet <IP> 23
```

En la pantalla de inicio de sesión del laboratorio, se usa el acceso indicado por el desafío.

## 3. Verificar privilegios

Una vez dentro, comprobá qué usuario sos y qué permisos tenés:

```bash
whoami
id
```

## 4. Localizar la evidencia

Seguí las instrucciones del laboratorio para ubicar y leer el archivo que confirma el desafío.

## Aprendizaje

- Telnet transmite todo sin cifrar, incluidas las credenciales. Por eso se considera inseguro.
- Lo recomendable es usar SSH y restringir el acceso remoto solo a las IP que lo necesitan.
- Antes de conectarte a un servicio, identificá qué protocolo es y qué protección tiene.
