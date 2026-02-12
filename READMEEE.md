# CFGS Desarrollo de Aplicaciones Web

|  CFGS DESARROLLO  DE APLICACIONES WEB |
|:-----------:|
|![Alt](webroot/images/portada.jpg)|
| DESPLIEGUE DE APLICACIONES WEB
| CIBERSEGURIDAD
| DWES Tema 2. INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DE ENTORNO DE DESARROLLO Y DEL ENTORNO DE EXPLOTACIÓN |

- [CFGS Desarrollo de Aplicaciones Web](#cfgs-desarrollo-de-aplicaciones-web)
  - [1.Entorno de Explotación](#1entorno-de-explotación)
    - [Acceso al panel Plesk](#acceso-al-panel-plesk)
    - [Gestión de archivos en EE](#gestión-de-archivos-en-ee)
    - [Conexión por SFTP y SSH](#conexión-por-sftp-y-ssh)
      - [SFTP](#sftp)
      - [SSH](#ssh)
    - [Creación de subdominios](#creación-de-subdominios)
    - [Añadir registros DNS para virtual hosts](#añadir-registros-dns-para-virtual-hosts)
    - [Despliegue desde GitHub](#despliegue-desde-github)
    - [Bases de datos](#bases-de-datos)
      - [Creación en Plesk](#creación-en-plesk)
      - [Importación](#importación)
      - [Configuración segura de la conexión a BBDD](#configuración-segura-de-la-conexión-a-bbdd)
    - [Archivo .htaccess](#archivo-htaccess)


## 1.Entorno de Explotación
El entorno de explotación es el sistema donde se publica la aplicación final accesible por los usuarios.
En este proyecto se utiliza Plesk como panel de control para la administración del hosting.

### Acceso al panel Plesk

El acceso se realiza mediante navegador web usando HTTPS y el puerto 8443:

> https://ieslossauces.es:8443


Tras introducir el usuario y la contraseña asignados, se accede al panel principal de gestión, desde donde se administran dominios, archivos, bases de datos y despliegues.

### Gestión de archivos en EE
Plesk permite administrar archivos directamente desde el navegador:

Ruta habitual:

`httpdocs/`

Desde el apartado Archivos se pueden:

1. Subir y descargar ficheros *(hacer con SFTP.)*
2. Crear carpetas
3. Editar archivos básicos
4. Eliminar contenido antiguo

![Alt](webroot/images/eear1.png)

### Conexión por SFTP y SSH
#### SFTP

Desde `Sitios web y dominios` → `FTP` se configuran:

- Usuario
- Contraseña
- Ruta de acceso
- Dirección IP del servidor

La conexión se realiza con clientes como MobaXTerm, permitiendo transferencias seguras de archivos.

#### SSH

Existen dos formas de acceso:

1. Terminal SSH integrada en Plesk
2. Cliente externo (MobaXTerm)

El acceso SSH permite ejecutar comandos según los permisos del usuario.

### Creación de subdominios
Desde `Sitios web y dominios → Añadir subdominio:`

![Alt](webroot/images/eesub1.png)\

1. Indicar el nombre del subdominio

2. Definir la raíz del documento (normalmente dentro de httpdocs)

3. Verificar que el nombre de la carpeta sea solo el del proyecto

![Alt](webroot/images/eesub2.png)

Cada subdominio suele corresponder a una aplicación o módulo independiente.

Configurar el nombre de nuestro subdominio y la ubicacion de los archivos

### Añadir registros DNS para virtual hosts
1. Acceder a `Hosting y DNS → DNS`
2. Añadir un nuevo registro
![Alt](webroot/images/vh1.png)
3. Tipo de registro: A
4. Nombre del subdominio
5. Dirección IP del servidor
6. Aplicar cambios y actualizar zona DNS

Esto permite que el subdominio sea accesible públicamente.

### Despliegue desde GitHub

Plesk permite enlazar repositorios Git directamente:

1. Acceder a Git dentro del dominio

2. Añadir repositorio usando URL SSH

3. Generar clave SSH desde Plesk

4. Añadir la clave en GitHub como Deploy Key

5. Activar despliegue automático

6. Elegir la carpeta destino dentro de `httpdocs`

Opcionalmente se puede configurar un Webhook para que cada push despliegue automáticamente el proyecto.

Es necesario que el subdominio tenga un certificado SSL válido.

**Publicación de versiones estables**

Alternativa al despliegue automático:

1. Crear una release estable en GitHub
2. Descargar el proyecto en formato ZIP
3. Extraerlo en local
4. Subirlo al servidor por SFTP
5. Ajustar configuraciones si es necesario

Método útil para entregas finales.

### Bases de datos
#### Creación en Plesk

Desde `Bases de datos → Añadir base de datos:`

1. Nombre de la base de datos
2. Subdominio asociado
3. Usuario y contraseña segura

Tras crearla, se habilita el acceso a phpMyAdmin.

#### Importación

Para cargar la estructura inicial:

1. Preparar scripts SQL
2. Comprimirlos en .zip
3. Importarlos desde Importar volcado

#### Configuración segura de la conexión a BBDD

Las credenciales no deben almacenarse en el código.

**Variables de entorno (`.htaccess`)**
```apache
SetEnv DB_DSN "mysql:host=localhost;dbname=DBProyecto"
SetEnv DB_USERNAME "usuarioDB"
SetEnv DB_PASSWORD "passwordSegura"
```
**Archivo PHP de conexión**
```php
define('DSN', getenv('DB_DSN'));
define('USERNAME', getenv('DB_USERNAME'));
define('PASSWORD', getenv('DB_PASSWORD'));
```
Este archivo no debe versionarse y se añade al `.gitignore`.

### Archivo .htaccess

Ejemplo de configuración habitual:
```apache
DirectoryIndex index.php

ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
ErrorDocument 500 /error/500.html

RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R,L]
```
Permite controlar redirecciones, errores personalizados y variables de entorno.

---

> **Cristian Mateos Vega**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  