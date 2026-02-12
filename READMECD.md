# CFGS Desarrollo de Aplicaciones Web

|  CFGS DESARROLLO  DE APLICACIONES WEB |
|:-----------:|
|![Alt](webroot/images/portada.jpg)|
| DESPLIEGUE DE APLICACIONES WEB
| CIBERSEGURIDAD
| DWES Tema 2. INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DE ENTORNO DE DESARROLLO Y DEL ENTORNO DE EXPLOTACIÓN |

- [CFGS Desarrollo de Aplicaciones Web](#cfgs-desarrollo-de-aplicaciones-web)
- [1. Cliente de Desarrollo](#1-cliente-de-desarrollo)
  - [1.1 Navegadores](#11-navegadores)
    - [1.1.1 Google Chrome](#111-google-chrome)
      - [Instalación](#instalación)
      - [Uso para desarrollo](#uso-para-desarrollo)
  - [1.2 MobaXTerm](#12-mobaxterm)
    - [Instalación](#instalación-1)
    - [Funcionalidades principales](#funcionalidades-principales)
    - [Ejemplo de uso](#ejemplo-de-uso)
  - [1.3 Netbeans](#13-netbeans)
    - [Crear Proyecto](#crear-proyecto)
    - [Eliminar Proyecto](#eliminar-proyecto)
    - [Control de versiones](#control-de-versiones)
    - [Debug](#debug)
    - [Conexión con bases de datos](#conexión-con-bases-de-datos)
  - [1.4 Visual Studio Code](#14-visual-studio-code)
    - [Crear Proyecto](#crear-proyecto-1)
      - [Workspaces (Espacios de trabajo)](#workspaces-espacios-de-trabajo)
    - [Eliminar Proyecto](#eliminar-proyecto-1)
    - [Control de versiones](#control-de-versiones-1)
    - [Sincronización SFTP con servidor de desarrollo](#sincronización-sftp-con-servidor-de-desarrollo)
    - [Debug con Xdebug](#debug-con-xdebug)
    - [Conexión con Bases de Datos](#conexión-con-bases-de-datos-1)
  - [1.5 PHP Documentor en el cliente](#15-php-documentor-en-el-cliente)

# 1. Cliente de Desarrollo

## 1.1 Navegadores
### 1.1.1 Google Chrome

Google Chrome es un navegador recomendado para desarrollo web por su compatibilidad con estándares modernos, herramientas de depuración y extensiones útiles para desarrolladores.

#### Instalación

Descargar desde: https://www.google.com/chrome/

Instalar normalmente en Windows.

#### Uso para desarrollo

- Inspección de elementos

Clic derecho → "Inspeccionar"

Permite ver la estructura HTML, CSS aplicado y manipularlo en tiempo real.

- Consola de JavaScript

Permite ejecutar código JS y ver errores del frontend.

- Herramientas de Red (Network)

Permite monitorizar peticiones HTTP, tiempos de respuesta y cabeceras.

- Depuración avanzada

Breakpoints en JS, edición de CSS en vivo, visualización de eventos y performance.

## 1.2 MobaXTerm
MobaXTerm es una herramienta todo en uno para conexión remota, transferencia de archivos y terminal avanzada.

### Instalación

Descargar desde: https://mobaxterm.mobatek.net/

Instalar versión Home o Portable en Windows 10.

### Funcionalidades principales

- SSH y SFTP integrados
- Conexión al servidor Ubuntu mediante SSH.
- Transferencia de archivos con SFTP         arrastrando y soltando.
- Terminal Unix/Linux
- Acceso a la línea de comandos con Bash, útil para ejecutar scripts, reiniciar servicios y editar ficheros de configuración.
- Gestión de sesiones
- Guardar múltiples conexiones con usuario y host preconfigurados.
- Permite ejecutar aplicaciones gráficas remotas en local.

### Ejemplo de uso

1. Abrir MobaXTerm → Quick Connect → SSH → introducir usuario y host.

2. Navegar hasta /var/www/html para gestionar proyectos.

3. Editar ficheros con el editor interno o abrirlos con NetBeans/VS Code.
## 1.3 Netbeans
### Crear Proyecto
1. En nuestro almacenamiento local, es decir, la maquina anfitriona (en mi caso Windows 10) creamos una carpeta para guardar nuestro proyecto\
![Alt](webroot/images/carpetalocal.png)
2. En nuestro almacenamiento remoto, es decir, nuestro servidor web en una máquina virtual de Ubuntu Server version 24.04.3 crearemos tambien un directorio en /var/www/html con el nombre del proyecto y dentro un index\
![Alt](webroot/images/directorioservidor.png)\
![Alt](webroot/images/indexproyectoprueba.png)
3. Ahora en Netbeans creamos el proyecto eligiendo el almacenamiento remoto de donde cogerá los archivos para empezar (el directorio con el index que creamos antes)
   1. Arriba a la izquierda damos a crear proyecto y luego elegimos PHP y la tercera opción\
![Alt](webroot/images/netbeanscp1.png)
   2. Ahora pondremos el nombre del proyecto y su ubicacion (la carpeta local de antes)\
![Alt](webroot/images/netbeanscp2.png)
   3. Ahora ponemos la url del servidor y el directorio de donde se cargaran los archivos (el index de antes)\
![Alt](webroot/images/netbeanscp3.png)
   4. Despues de dar varias veces a siguiente ya nos aparecerá el proyecto creado a la izquierda y tendremos el index creado en el servidor accesible desde el IDE\
![Alt](webroot/images/netbeanscp4.png)

### Eliminar Proyecto
1. Para eliminar un proyecto haremos clic derecho sobre el proyecto que queremos eliminar y ahí le daremos a "Delete".\
![Alt](webroot/images/netbeansdp1.png)
2. Se nos abrirá una ventana de confirmación que tendrá una opción para marcar por si queremos eliminar tambien la carpeta y los archivos del proyecto,elegimos esa opcion si queremos.\
![Alt](webroot/images/netbeansdp2.png)
1. Ahora en nuestro almacenamiento local, donde creamos el proyecto, ya no nos aparecerá, ya que se ha eliminado.
2. Pero aún conservaremos la carpeta y los archivos en remoto, los cuales si queremos podemos reutilizar para volver a crear un proyecto con ellos, o eliminarlos tambien.

### Control de versiones
1. Abrimos nuestro proyecto en Netbeans.
2. Hacemos click derecho en el proyecto y en el apartado "Versioning" le damos a inicializar repositorio.\
![Alt](webroot/images/netbeansgit1.png)
3. Hacer commit.\
![Alt](webroot/images/netbeansgit2.png)
![Alt](webroot/images/netbeansgit3.png)
4. Ahora estaremos en la rama master, para crear y cambiar ramas.\
![Alt](webroot/images/netbeansgit4.png)
![Alt](webroot/images/netbeansgit6.png)
![Alt](webroot/images/netbeansgit5.png)
![Alt](webroot/images/netbeansgit7.png)
5. Clonar repositorio.\
![Alt](webroot/images/netbeansgit8.png)
![Alt](webroot/images/netbeansgit9.png)
Luego se eligen las ramas para clonar y se le pone un nombre al nuevo proyecto clonado.
6. Hacer Push y Pull.\
![Alt](webroot/images/netbeansgit10.png)
![Alt](webroot/images/netbeansgit11.png)
Tanto para Pull como para Push la ventana que se abrirá será la misma, primero se elige la ruta del repositorio al que se va a subir o se va a descargar el contenido, luego las ramas remotas en las que está ese contenido y por ultimo, en el case de push las ramas locales de donde sale el contenido.
### Debug
1. Configurar en NetBeans: Tools → Options → PHP → Debugging:

        Debugger port: 9003 (para XDebug).

        Session ID: netbeans-xdebug.

2. Puntos de interrupción (breakpoints) sobre líneas de código PHP.

3. Ejecutar el proyecto en modo Debug → observar variables, call stack y valores en tiempo real.
### Conexión con bases de datos
1. Ir a la pestaña services y hacer click derecho en Database > New Connection...\
![Alt](webroot/images/netbeansdb1.png)
2. Elegir driver\
![Alt](webroot/images/netbeansdb2.png)
3. Configurar los datos de la base de datos como el usuario, el host...\
![Alt](webroot/images/netbeansdb3.png)

## 1.4 Visual Studio Code
### Crear Proyecto

Crear carpeta local en Windows 10.

Abrir VS Code → File → Open Folder → seleccionar carpeta del proyecto.

Instalar extensiones recomendadas:

PHP Intelephense

PHP Debug

GitLens

#### Workspaces (Espacios de trabajo)

Un workspace permite trabajar con varios proyectos en una sola ventana, manteniendo:

- Configuración común
- Extensiones
- Rutas y depuración centralizada

Se guarda en un archivo `.code-workspace` (JSON), lo que facilita reutilizarlo y compartirlo.

**Creación del workspace**

Abrir una ventana vacía

> Ctrl + Shift + N


Añadir carpetas al workspace

> File → Add Folder to Workspace…


Estructura recomendada:
```makefile
D:\
└── ProyectosWebDAW
    ├── ProyectoDAW
    ├── ProyectoDWEC
    ├── ProyectoDWES
    └── ...
```
Se añaden las carpetas de proyecto, no la carpeta contenedora.

**Guardar el workspace**

> File → Save Workspace As…


Importante para poder usar configuraciones avanzadas (debug, launch, etc.).

### Eliminar Proyecto

Cerrar VS Code → eliminar carpeta del proyecto desde el explorador de Windows.

Si el proyecto está versionado con Git, también se puede eliminar local y mantener remoto.

### Control de versiones

El control de versiones se gestiona desde el panel `Source Control`:

> Ctrl + Shift + G

Desde ahí se puede:

- Hacer commits
- Cambiar ramas
- Crear tags
- Gestionar stashes

`.gitignore` recomendado
```pgsql
.vscode/
.vscode-server/
*.code-workspace

.cache/
.local/
.ssh/

.bashrc
.bash_history

nbproject/
```
### Sincronización SFTP con servidor de desarrollo

Cada proyecto debe tener una carpeta `.vscode/` con el archivo sftp.json.

*Se puede crear una vez y reutilizar en otros proyectos.*

Estructura básica de `sftp.json`
```json
{
  "name": "ConexionServidor",
  "context": ".",
  "host": "IP_SERVIDOR",
  "username": "usuario",
  "password": "password",
  "remotePath": "/ruta/remota/proyecto",
  "uploadOnSave": true,
  "ignore": [
    ".git",
    ".DS_Store"
  ]
}
```
**Sincronización manual**

Desde el explorador:

> Sync Local → Remote → Subir archivos

> Sync Remote → Local → Descargar archivos

> Sync Both Directions → Sincronización bidireccional

Archivos externos (imágenes, PDFs, etc.) suelen requerir subida manual.

### Debug con Xdebug

**Configuración previa**

En los ajustes de VSCode:
```ini
php.debug.idekey = netbeans-xdebug
```
**Configuración del workspace**

En el archivo `.code-workspace`, al mismo nivel que "`folders`", añadir:
```json
{
  "launch": {
    "version": "0.2.0",
    "configurations": [
      {
        "name": "Listen for Xdebug",
        "type": "php",
        "request": "launch",
        "port": 9003,
        "pathMappings": {
          "/var/www/daw211/httpdocs": "${workspaceFolder:ProyectoDAW}"
        }
      }
    ]
  }
}
```

Ajustar las rutas remotas ↔ locales según el proyecto.

**Uso del debugger**
1. Abrir el panel de depuración
> Ctrl + Shift + D
2. Ejecutar `Listen for Xdebug`
3. Colocar breakpoints y cargar la página PHP

### Conexión con Bases de Datos

Abrir el panel de bases de datos (icono de cilindro)

En **Connections** → **Add New Connection**

Seleccionar **MariaDB**

Completar los datos de conexión

Para acceso completo:
```makefile
Database: mysql
```
**Ejecución de consultas**

Seleccionar la conexión activa desde la barra inferior

Abrir un archivo `.sql`

Ejecutar:

Consulta seleccionada → **Ctrl + E (dos veces)**

Todo el archivo → **Ctrl + A + Ctrl + E (dos veces)**

## 1.5 PHP Documentor en el cliente
**1. Requisitos del sistema**

- Sistema operativo: Windows 10
- Editor: Visual Studio Code
- PHP instalado (uso por CLI)
- Archivo `phpDocumentor.phar`

---

**2. Comprobación de PHP**

Abrir una terminal (CMD o PowerShell) y ejecutar:

```bash
php -v
```
Debe mostrarse la versión de PHP instalada.\
Para conocer la ruta del ejecutable:
```bash
where php
```

Ejemplo de salida:
```
C:\php\php.exe
```

**3. Activar el archivo php.ini**

Si al ejecutar:

```
php --ini
```

no aparece ningún archivo php.ini, significa que PHP no está cargando configuración.

*Solución*

Acceder a la carpeta donde está PHP (por ejemplo C:\php)

Copiar uno de los archivos:

`php.ini-development` (recomendado para desarrollo)

`php.ini-production`

Renombrarlo a:

`php.ini`

**4. Configuración correcta del php.ini**

Abrir php.ini y comprobar las siguientes líneas:

```ini
extension_dir = "C:\php\ext"
extension=mbstring
```

Guardar el archivo.\
Cerrar y volver a abrir la terminal.

Verificar:

```
php -m
```

Debe aparecer:\
`mbstring`

**5. Instalación y uso de phpDocumentor**

Colocar el archivo phpDocumentor.phar en el proyecto o en una carpeta accesible.

Ejemplo de ejecución desde la raíz del proyecto:

```
php phpDocumentor.phar run -d ./ -t ./docs
```

Parámetros:

`-d ./ → código fuente`\
`-t ./docs → carpeta destino de la documentación`

**6. Carpeta .phpdoc/cache**

Durante la ejecución se genera automáticamente:

`.phpdoc/cache`

Se puede ignorar o añadir al .gitignore

**7. Uso con Visual Studio Code**

Abrir el proyecto como carpeta completa

Usar la terminal integrada

Ejecutar phpDocumentor desde VS Code

Abrir la documentación en:

`docs/index.html`

---

> **Cristian Mateos Vega**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  