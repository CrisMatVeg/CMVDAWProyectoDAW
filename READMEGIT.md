# CFGS Desarrollo de Aplicaciones Web

|  CFGS DESARROLLO  DE APLICACIONES WEB |
|:-----------:|
|![Alt](webroot/images/portada.jpg)|
| DESPLIEGUE DE APLICACIONES WEB
| CIBERSEGURIDAD
| DWES Tema 2. INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DE ENTORNO DE DESARROLLO Y DEL ENTORNO DE EXPLOTACIÓN |

- [CFGS Desarrollo de Aplicaciones Web](#cfgs-desarrollo-de-aplicaciones-web)
  - [1. Git](#1-git)
    - [Instalación](#instalación)
    - [Verificación:](#verificación)
    - [Configuración inicial](#configuración-inicial)
    - [Flujo de trabajo básico](#flujo-de-trabajo-básico)
    - [Ramas y estrategia de trabajo](#ramas-y-estrategia-de-trabajo)
    - [Buenas prácticas de commits](#buenas-prácticas-de-commits)
      - [Formato:](#formato)
      - [Reglas clave:](#reglas-clave)
      - [Deshacer cambios](#deshacer-cambios)
      - [Etiquetas (tags)](#etiquetas-tags)
  - [2. GitHub](#2-github)
    - [Requisitos previos](#requisitos-previos)
    - [Crear un repositorio en GitHub](#crear-un-repositorio-en-github)
    - [Clonar un repositorio en tu máquina local](#clonar-un-repositorio-en-tu-máquina-local)
    - [Conectar repositorio local con GitHub](#conectar-repositorio-local-con-github)
    - [Añadir archivos y hacer commits](#añadir-archivos-y-hacer-commits)
    - [Enviar cambios al repositorio remoto (push)](#enviar-cambios-al-repositorio-remoto-push)
    - [Obtener cambios del repositorio remoto (pull)](#obtener-cambios-del-repositorio-remoto-pull)
    - [Crear Releases](#crear-releases)
    - [Otras funciones](#otras-funciones)
      - [Ver el estado de los archivos:](#ver-el-estado-de-los-archivos)
      - [Ver historial de commits:](#ver-historial-de-commits)
      - [Crear una nueva rama:](#crear-una-nueva-rama)
      - [Cambiar de rama:](#cambiar-de-rama)
    - [Conexión son SSH](#conexión-son-ssh)
    - [Trabajo colaborativo](#trabajo-colaborativo)

## 1. Git
**¿Qué es Git?**

Git es un sistema de control de versiones distribuido que permite:

- Registrar cambios en el código
- Volver a versiones anteriores
- Trabajar con ramas
- Colaborar sin sobrescribir trabajo de otros

Git funciona en local, sin necesidad de conexión a internet.

### Instalación

Descarga desde la web oficial: https://git-scm.com/

**Durante la instalación:**

- Editor por defecto: Visual Studio Code

- Rama inicial: master (o main según el proyecto)

- PATH: Git from command line and 3rd-party software

- Resto de opciones: valores por defecto

### Verificación:
```bash
git --version
```
### Configuración inicial

Antes de empezar, es obligatorio identificar al autor de los commits:
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

**Comprobar configuración:**
```bash
git config --list
```
**Estructura básica de un repositorio**

Un repositorio Git contiene una carpeta oculta:

`.git/`


Esta carpeta guarda:

- Historial de commits
- Ramas
- Configuración
- Referencias a repositorios remotos

> Si se elimina .git, el proyecto deja de estar versionado.

### Flujo de trabajo básico

- Modificar archivos
- Añadir cambios al área de preparación (staging)
- Confirmar cambios (commit)
- Subir cambios al remoto (opcional)

**Comandos principales:**
```bash
git status
git add .
git commit -m "mensaje"
```
### Ramas y estrategia de trabajo

Las ramas permiten trabajar en paralelo sin afectar al código principal.

**Comandos habituales:**
```bash
git branch
git checkout nombre-rama
git checkout -b nueva-rama
```

**Estrategia recomendada:**

`master / main`: versión estable

`develop`: desarrollo

`ramas feature`: nuevas funcionalidades

### Buenas prácticas de commits

Se recomienda usar Commits Convencionales:

#### Formato:

`tipo(scope)`: descripción

Ejemplos:

> `feat(login)`: añadir validación

> `fix(api)`: corregir error 500

> `docs(readme)`: actualizar instrucciones

#### Reglas clave:

- Mensajes cortos (≤ 50 caracteres)
- Lenguaje imperativo
- Sin punto final
- Un commit = un cambio lógico

#### Deshacer cambios

Deshacer cambios no confirmados:
```bash
git restore archivo
```

Quitar del staging:
```bash
git reset archivo
```

Deshacer commits:
```bash
git reset --soft HEAD~1
git reset --hard HEAD~1
```

Usar `--hard` elimina los cambios del disco.

#### Etiquetas (tags)

Las etiquetas marcan versiones importantes del proyecto.

**Crear etiqueta:**
```bash
git tag v1.0.0
git tag -a v1.1.0 -m "Versión estable"
```

**Subir etiquetas:**
```bsah
git push origin --tags
```
## 2. GitHub
**¿Qué es GitHub?**

GitHub es una plataforma online que aloja repositorios Git y añade:

- Colaboración
- Pull Requests
- Issues
- Releases
- Automatización (CI/CD)

### Requisitos previos
1. Tener una cuenta en [GitHub](https://github.com/)
2. Instalar [Git](https://git-scm.com/) en tu sistema
3. Configurar Git con tus credenciales: *Ver en el apartado Git*

### Crear un repositorio en GitHub
1. Inicia sesión en GitHub
2. Haz clic en el botón New (nuevo repositorio)\
![Alt](webroot/images/github1.png)
3. Asigna un nombre al repositorio
4. Opcional: añade una descripción, README, .gitignore y licencia
5. Haz clic en Create repository\
![Alt](webroot/images/github2.png)

### Clonar un repositorio en tu máquina local
```bash
# Esto crea una copia local del repositorio remoto.
git clone https://github.com/usuario/repositorio
```
### Conectar repositorio local con GitHub
```bash
git remote add origin https://github.com/usuario/repositorio.git
git push -u origin master
```

Comprobar remotos:
```bash
git remote -v
```
### Añadir archivos y hacer commits
1. Añade o modifica archivos en tu carpeta local
2. Añade los archivos al área de preparación:

```bash
git add archivo.txt
# o para todos los archivos
git add .
# Realiza un commit con un mensaje descriptivo:
git commit -m "Descripción del cambio"
```

### Enviar cambios al repositorio remoto (push)
```bash
# Reemplaza main por la rama que estés usando si es diferente.
git push origin main
```

### Obtener cambios del repositorio remoto (pull)
```bash
# Esto sincroniza tu copia local con los cambios del repositorio remoto.
git pull origin main
```
**Descargar sin fusionar:**
```bash
git fetch
```
### Crear Releases
Un release representa una versión pública del proyecto.

**Proceso:**

1. Crear un tag
2. Ir a Releases en GitHub
3. Crear nueva release
4. Asociar el tag
5. Añadir descripción de cambios

Sirve para:

- Versionado
- Descargas
- Historial estable
### Otras funciones
#### Ver el estado de los archivos:
```bash
git status
```
#### Ver historial de commits:
```bash
git log
```
#### Crear una nueva rama:
```bash
git checkout -b nombre-rama
```
#### Cambiar de rama:
```bash
git checkout nombre-rama
```

### Conexión son SSH
1. Generar clave:
```bash
ssh-keygen -t ed25519 -C "tu@email.com"
```

2. Añadir clave al agente:
```bash
ssh-add ~/.ssh/id_ed25519
```

3. Copiar la clave pública (.pub) y añadirla en:

> GitHub → Settings → SSH and GPG keys


**Ventajas:**

- Más seguro
- No requiere contraseña en cada push

### Trabajo colaborativo

**Flujo recomendado:**

1. Clonar repositorio
2. Crear rama
3. Realizar cambios
4. Push de la rama
5. Crear Pull Request
6. Revisión y merge
7. Protección de ramas

**En GitHub:**

> Settings → Branches → Branch protection rules


Opciones recomendadas:

> - [X] Require pull request before merging

> - [X] Require linear history

> - [ ] Allow force pushes

Esto evita cambios directos en la rama principal.

---

> **Cristian Mateos Vega**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  