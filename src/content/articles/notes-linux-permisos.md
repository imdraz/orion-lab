---
title: "Linux: permisos de archivos explicados"
description: "Cómo leer los permisos rwx, qué significan para usuario, grupo y otros, y cómo cambiarlos con chmod y chown."
pubDate: 2026-11-01
category: "notes"
draft: false
---

En Linux, cada archivo tiene un dueño, un grupo y un conjunto de permisos. Entenderlos es clave para la seguridad: un permiso mal configurado puede dar acceso a quien no debería.

## Cómo se leen

```bash
ls -l /etc/passwd
-rw-r--r-- 1 root root 2847 ... /etc/passwd
```

La primera letra indica el tipo (`-` archivo, `d` directorio). Luego vienen tres bloques de tres caracteres:

| Bloque | Aplica a | Ejemplo |
|---|---|---|
| `rw-` | El dueño | lectura y escritura |
| `r--` | El grupo | solo lectura |
| `r--` | Otros usuarios | solo lectura |

Cada letra significa:

- `r` (read): leer, valor 4.
- `w` (write): modificar, valor 2.
- `x` (execute): ejecutar, valor 1. En directorios, permite entrar.

## Notación numérica

Se suman los valores. Por ejemplo, `rw-r--r--` equivale a `644`, y `rwxr-x---` equivale a `750`.

## Cambiar permisos

```bash
chmod 640 informe.txt       # dueño rw, grupo r, otros sin acceso
chmod u+x script.sh         # agrega ejecución para el dueño
```

## Cambiar dueño y grupo

```bash
sudo chown alumno:alumnos informe.txt
```

## Buenas prácticas

1. Aplicar el **mínimo privilegio**: solo los permisos necesarios.
2. Evitar `777`: da control total a cualquier usuario del sistema.
3. Revisar periódicamente archivos con permisos de escritura para todos, con `find / -perm -o+w -type f 2>/dev/null`.
4. Tener cuidado con los bits SUID y SGID, porque permiten ejecutar un programa con los privilegios de su dueño.
