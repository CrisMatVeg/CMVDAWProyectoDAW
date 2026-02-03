# CFGS Desarrollo de Aplicaciones Web

|  CFGS DESARROLLO  DE APLICACIONES WEB |
|:-----------:|
|![Alt](webroot/images/portada.jpg)|
| DESPLIEGUE DE APLICACIONES WEB
| CIBERSEGURIDAD
| DWES Tema 2. INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DE ENTORNO DE DESARROLLO Y DEL ENTORNO DE EXPLOTACIÓN |

- [CFGS Desarrollo de Aplicaciones Web](#cfgs-desarrollo-de-aplicaciones-web)
    - [1.2 Cliente de Desarrollo](#12-cliente-de-desarrollo)
      - [1.2.2 **Navegadores**](#122-navegadores)
      - [1.2.3 **MobaXTerm**](#123-mobaxterm)
      - [1.2.4 **Netbeans**](#124-netbeans)
        - [**Crear Proyecto**](#crear-proyecto)
        - [**Eliminar Proyecto**](#eliminar-proyecto)
        - [**Control de versiones**](#control-de-versiones)
        - [**Debug**](#debug)
        - [**Conexión con bases de datos**](#conexión-con-bases-de-datos)
      - [1.2.5 **Visual Studio Code**](#125-visual-studio-code)
        - [PHP Documentor en el cliente](#php-documentor-en-el-cliente)

### 1.2 Cliente de Desarrollo

#### 1.2.2 **Navegadores**
#### 1.2.3 **MobaXTerm**
#### 1.2.4 **Netbeans**
##### **Crear Proyecto**
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

##### **Eliminar Proyecto**
1. Para eliminar un proyecto haremos clic derecho sobre el proyecto que queremos eliminar y ahí le daremos a "Delete".\
![Alt](webroot/images/netbeansdp1.png)
2. Se nos abrirá una ventana de confirmación que tendrá una opción para marcar por si queremos eliminar tambien la carpeta y los archivos del proyecto,elegimos esa opcion si queremos.\
![Alt](webroot/images/netbeansdp2.png)
1. Ahora en nuestro almacenamiento local, donde creamos el proyecto, ya no nos aparecerá, ya que se ha eliminado.
2. Pero aún conservaremos la carpeta y los archivos en remoto, los cuales si queremos podemos reutilizar para volver a crear un proyecto con ellos, o eliminarlos tambien.

##### **Control de versiones**
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
##### **Debug**
##### **Conexión con bases de datos**
1. Ir a la pestaña services y hacer click derecho en Database > New Connection...\
![Alt](webroot/images/netbeansdb1.png)
2. Elegir driver\
![Alt](webroot/images/netbeansdb2.png)
3. Configurar los datos de la base de datos como el usuario, el host...\
![Alt](webroot/images/netbeansdb3.png)

#### 1.2.5 **Visual Studio Code**
##### PHP Documentor en el cliente
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