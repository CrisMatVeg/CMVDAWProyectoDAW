# CFGS Desarrollo de Aplicaciones Web

|  CFGS DESARROLLO  DE APLICACIONES WEB |
|:-----------:|
|![Alt](webroot/images/portada.jpg)|
| DESPLIEGUE DE APLICACIONES WEB
| CIBERSEGURIDAD
| DWES Tema 2. INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DE ENTORNO DE DESARROLLO Y DEL ENTORNO DE EXPLOTACIÓN |

- [CFGS Desarrollo de Aplicaciones Web](#cfgs-desarrollo-de-aplicaciones-web)
  - [1. Entorno de Desarrollo](#1-entorno-de-desarrollo)
    - [1.1 Sistema Operativo Ubuntu Server 24.04 LTS](#11-sistema-operativo-ubuntu-server-2404-lts)
      - [Configuración inicial](#configuración-inicial)
        - [Nombre y configuración de red](#nombre-y-configuración-de-red)
        - [Actualizar el sistema](#actualizar-el-sistema)
        - [Configuración fecha y hora](#configuración-fecha-y-hora)
        - [Cuentas administradoras](#cuentas-administradoras)
        - [Habilitar cortafuegos](#habilitar-cortafuegos)
          - [Monitorizacion](#monitorizacion)
          - [Mantenimiento](#mantenimiento)
  - [1.2 Servicios](#12-servicios)
    - [1.2.1 Apache HTTP](#121-apache-http)
      - [Instalación](#instalación)
        - [Verficación del servicio](#verficación-del-servicio)
      - [Configuración](#configuración)
        - [Permisos y usuarios](#permisos-y-usuarios)
        - [ErrorDocument](#errordocument)
        - [Redirección HTTP a HTTPS](#redirección-http-a-https)
      - [Monitorización](#monitorización)
      - [Mantenimiento](#mantenimiento-1)
    - [1.2.2 PHP](#122-php)
      - [Instalación antigua](#instalación-antigua)
      - [Instalación nueva](#instalación-nueva)
        - [Configuración de Apache2 con PHP-FPM](#configuración-de-apache2-con-php-fpm)
        - [Activarlo para todos los virtualhost](#activarlo-para-todos-los-virtualhost)
      - [Módulos PHP](#módulos-php)
        - [Como instalar módulos](#como-instalar-módulos)
        - [Verificación de módulos activos](#verificación-de-módulos-activos)
      - [Configuración PHP](#configuración-php)
      - [Entorno de desarrollo PHP](#entorno-de-desarrollo-php)
      - [Entorno de explotación PHP](#entorno-de-explotación-php)
      - [Monitorización PHP](#monitorización-php)
      - [Mantenimiento PHP](#mantenimiento-php)
    - [1.2.3 MariaDB](#123-mariadb)
      - [Instalación](#instalación-1)
      - [Configuración](#configuración-1)
    - [1.2.4 XDebug](#124-xdebug)
      - [Instalación](#instalación-2)
      - [Configuración](#configuración-2)
    - [1.2.5 Apache HTTPS](#125-apache-https)
    - [1.2.6 DNS](#126-dns)
        - [Virtual Hosts](#virtual-hosts)
    - [1.2.7 SFTP](#127-sftp)
      - [Enjaulado de usuarios](#enjaulado-de-usuarios)
    - [1.2.8 Apache Tomcat](#128-apache-tomcat)
    - [1.2.9 LDAP](#129-ldap)
  - [2. Documentación de Código](#2-documentación-de-código)
    - [2.1 PHPDoc / phpDocumentor](#21-phpdoc--phpdocumentor)

---

## 1. Entorno de Desarrollo
Este documento es una guía detallada del proceso de instalación y configuración de un servidor de aplicaciones en Ubuntu Server utilizando Apache, con soporte PHP y MySQL

### 1.1 Sistema Operativo Ubuntu Server 24.04 LTS

```bash
#Para ver version del sistema:
uname -a
```
#### Configuración inicial
##### Nombre y configuración de red

> **Nombre de la máquina**: CMV-USLimpia
```bash
#Se ve con: 
hostname

#Se cambia con:
1. sudo hostnamectl set-hostname "nombre"
2. sudo nano /etc/hosts y cambiando la linea donde pone 127.0.1.1
```
![Alt](webroot/images/hostname1.png)

> **Memoria RAM**: 2G\
> **Particiones**: 150G(/) y resto (/var)\
```bash
#Se ve con:
df -h
```
![Alt](webroot/images/df1.png)
> **Configuración de red interface**: enp0s3\
> **Dirección IP** :10.199.8.248/22\
> **GW**: 10.199.8.1/22\
> **DNS**: 10.151.123.21 y 10.151.126.21
```bash
#Se ve con:
ip a (RED)
ip r (GW)
resolvectl (DNS)

#Se cambia entrando en:
sudo nano /etc/netplan/enp0s3.yaml
(Ver más abajo)
```

Editar el fichero de configuración del interface de red  **/etc/netplan**,
```bash
#CONFIGURACION DINAMICA
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
````

```bash
#CONFIGURACION ESTATICA
network:
  ethernets:
    enp0s3:
      addresses:
       - 10.199.8.248/22
      nameservers:
         addresses:
         - 10.151.123.21
         - 10.151.126.21
         search: [educa.jcyl.es]
      routes:
        - to: default
          via: 10.199.8.1
            version: 2
```

##### Actualizar el sistema

```bash
sudo apt update
sudo apt upgrade
```
##### Configuración fecha y hora

[Establecer fecha, hora y zona horaria](https://somebooks.es/establecer-la-fecha-hora-y-zona-horaria-en-la-terminal-de-ubuntu-20-04-lts/ "Cambiar fecha y hora")


##### Cuentas administradoras

> - [X] root(inicio)
> - [X] miadmin/paso
> - [X] miadmin2/paso

```bash
#Los usuarios se ven en:
sudo cat /etc/passwd [|grep "nombre de usuario para filtrar"]
#Para ver los usuarios por grupo:
sudo cat /etc/group
```
##### Habilitar cortafuegos
```bash
#Para encender cortafuegos:
sudo ufw start

#Para habilitar puertos:
sudo ufw allow 22

#Para ver estado y puertos activos:
sudo ufw status [numbered]

#Para eliminar algún puerto
sudo ufw delete "nº de puerto (ver con numbered)"
``` 
###### Monitorizacion
```bash
sudo ufw status verbose    # Mostramos el estado detallado del cortafuegos y las reglas activas
```
###### Mantenimiento
```bash
# Desactivamos el cortafuegos temporalmente
sudo ufw disable
# Reseteamos todas las reglas a la configuración inicial           
sudo ufw reset             
```

---

## 1.2 Servicios

### 1.2.1 Apache HTTP

#### Instalación
```bash
sudo apt install apache2
sudo ufw allow 80
```
##### Verficación del servicio
```bash
sudo service apache2 status
sudo systemctl status apache2
```

#### Configuración

##### Permisos y usuarios
```bash
#Para crear el usuario y asignarle una contraseña
sudo useradd -M -d /var/www/html -s /bin/bash -g www-data operadorweb
sudo passwd operadorweb

#Para comprobar que se ha creado bien:
cat /etc/passwd | grep operadorweb

#Cambiar propietario de grupo:
sudo chown -R operadorweb:www-data /var/www/html

#Cambiar permisos de la carpeta:
sudo chmod -R 775 /var/www/html
```
Los archivos de configuracion de Apache se encuentran en **``/etc/apache2/``**:
  - **``apache2.conf``**: es el archivo de configuracion inicial. Es el primer fichero que se ejecuta cuando arrancamos el servidor.
  - **``ports.conf``**: donde se definen los puertos en los que Apache escuchará las conexiones
  - **``mods-available/``**: Contiene todos los módulos de Apache que están instalados en el sistema.
  - **``mods-enabled/``**: Contiene enlaces simbólicos a los módulos de mods-available/ que están activos, es decir, cargados y funcionando en el servidor.
  - **``conf-available/``**: Almacena archivos de configuración global mediante enlaces simbólicos influyendo en la configuración general del servidor.
  - **``conf-enabled/``**: Contiene enlaces simbólicos a los archivos de conf-available/ que están activos, aplicando su configuración al servidor.
  - **``sites-available/``**: Guarda archivos de configuración de sitios virtuales mediante enlaces simbólicos, permitiendo configurar diferentes sitios alojados en el mismo servidor
  - **``sites-enabled/``**: Contiene enlaces simbólicos a los archivos de sites-available/ que están activos, habilitando los sitios virtuales correspondientes.

##### ErrorDocument
```bash
# Para cada error tendremos que poner la siguiente linea en el .htaccess de nuestra página, que se encuentra en /var/www/html
ErrorDocument "Codigo de Error" /error/"archivo para ese error".html
# El directorio error ha sido creado previamente para poner ahí las paginas que se mostrarán cuando ocurran esos errores
```
Despues de incluir varios errores quedaría algo así\
![Alt](webroot/images/errordocument1.png)

##### Redirección HTTP a HTTPS
```bash
# Primero habilitamos el modulo rewrite
sudo a2enmod rewrite
#Luego vamos al .htacces de nuestra pagina, se encuentra en /var/www/html y ponemos lo siguiente
RewriteEngine On 
RewriteCond %{SERVER_PORT} 80 
RewriteRule ^(.*)$ https://www.dominio.com/$1 [R,L]
```
Quedará algo así\
![Alt](webroot/images/redirect1.png)\
Explicación de las directivas de .httaccess
- **Options +FollowSymLinks** - es una directiva de Apache, requisito previo para mod_rewrite. 
- **RewriteEngine On** - habilita mod_rewrite. 
- **RewriteCond  %{SERVER_PORT}  80** - sirve para indicar que todas las peticiones que se realicen al puerto 80 (puerto por defecto de Apache para servicio web), deseamos que vayan a través de la regla especificada. 
- **RewriteRule** - define una regla particular. 
Dentro de la regla de reescritura, la primera cadena de caracteres después de RewriteRule, define lo que la URL original parece. 
La segunda cadena después de RewriteRule define la nueva URL. 

  - **$1** - Este carácter especial, sustituye (o indica) la parte entre paréntesis, especificada en la primera cadena. Básicamente, lo que haces es asegurar que las sub-páginas redireccionan a la misma sub-página y no a la página principal. Puede omitirlo para redirigir a la página principal. (Si usted no tiene el mismo contenido en el nuevo directorio que había en el antiguo directorio, deje esta expresión regular 

  - **[R,L]**- Esta opción, realiza una redirección, y también deshabilita que las reglas de reescritura que estén escritas después afecten a la dirección URL (una buena idea para añadir después de la última rewrite rule).

#### Monitorización
Apache genera ficheros de log que permiten supervisar el funcionamiento del servidor y depurar problemas:

Ficheros principales de log:

Accesos: `/var/log/apache2/access.log`
Registra todas las solicitudes HTTP recibidas por el servidor, con información como IP, fecha, método, URL, código de respuesta y user-agent.
```bash
tail -f /var/log/apache2/access.log
```

Errores: `/var/log/apache2/error.log`
Registra errores de configuración, fallos en scripts PHP, problemas con módulos o permisos.
```bash
tail -f /var/log/apache2/error.log
```
Monitorización en tiempo real:

Comprobar actividad en tiempo real:
```bash
sudo tail -f /var/log/apache2/access.log /var/log/apache2/error.log
```

Revisar el uso de procesos Apache:
```bash
ps aux | grep apache2
```

Comprobación del estado del servicio:
```bash
sudo systemctl status apache2
sudo apache2ctl configtest  # Verifica la sintaxis de la configuración
```

#### Mantenimiento
Reiniciar Apache: Para aplicar cambios de configuración o módulos
```bash
sudo systemctl restart apache2
```

Recargar configuración sin interrumpir conexiones activas:
```bash
sudo systemctl reload apache2
```

Habilitar/Deshabilitar módulos:
```bash
sudo a2enmod nombre_modulo
sudo a2dismod nombre_modulo
sudo systemctl reload apache2
```

Backup de configuración:
```bash
sudo cp -r /etc/apache2 /etc/apache2_backup_$(date +%F)
```
---

### 1.2.2 PHP

#### Instalación antigua
```bash
#Instala herramientas para administrar los repositorios de software, como add-apt-repository
sudo apt install software-properties-common -y
 
#Añade un repositorio que proporciona versiones actualizadas de PHP (incluyendo 8.3).
sudo add-apt-repository ppa:ondrej/php -y

#Muestra los archivos de configuración de los repositorios y filtra los que contienen "ondrej". Sirve como verificación de que el repositorio se agregó correctamente
ls /etc/apt/sources.list.d/ | grep ondrej
 
#Actualiza la base de datos local de paquetes con la nueva información del repositorio ondrej/php.
sudo apt update
 
#Instala: 
#libapache2-mod-php8.3: módulo clásico de PHP para Apache. (OPCIONAL, mejor no instalarlo para evitar conflictos)
#php8.3-fpm: versión de PHP como servicio FPM (mejor para rendimiento y aislamiento).
sudo apt install libapache2-mod-php8.3 php8.3-fpm -y
 
#Desactiva mod_php, el método tradicional en que Apache ejecuta PHP. Esto es necesario para evitar conflictos con FPM.
sudo a2dismod php8.3
 
#mpm_prefork es el módulo de procesamiento que se usa con mod_php. Incompatible con proxy_fcgi, por eso se desactiva.
sudo a2dismod mpm_prefork
 
#mpm_event: MPM más moderno y eficiente, compatible con php-fpm. proxy_fcgi: Asegura que Apache pueda comunicarse con PHP-FPM.
sudo a2enmod mpm_event proxy_fcgi
 
#Activa el archivo de configuración que conecta Apache con el servicio php8.3-fpm.
sudo a2enconf php8.3-fpm
 
#Reinicia Apache para que los cambios surtan efecto.
sudo systemctl restart apache2
```

#### Instalación nueva
```bash
sudo apt install php8.3-fpm php8.3
sudo systemctl restart php8.3-fpm
```

##### Configuración de Apache2 con PHP-FPM
---

Para habilitar los modulos Proxy_FCGI y SetEnvif
```bash
sudo a2enmod proxy_fcgi setenvif
```

##### Activarlo para todos los virtualhost

El fichero de configuración php8.3-fpm en el directorio /etc/apache2/conf-available, por defecto funciona cuando php-fpm está escuchando en un socket UNIX
Se configura de la siguiente forma:

Se abre el fichero /etc/apache2/conf-available/php8.3-fpm.conf

```bash
sudo nano /etc/apache2/conf-available/php8.3-fpm.conf
``` 
Y se copia esto o se comprueba si ya está:
```bash
 <FilesMatch ".+\.ph(?:ar|p|tml)$">
    SetHandler "proxy:unix:/run/php/php8.3-fpm.sock|fcgi://localhost"
</FilesMatch>
```   
  
Por último activamos y recargamos apache2:

```bash
sudo a2enconf php8.3-fpm
systemctl reload apache2
```
#### Módulos PHP
| Módulo             | Descripción                                                                                                       | Entorno recomendado                    |
| ------------------ | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| `php8.3-intl`      | Internacionalización: formatea fechas, monedas y cadenas según localización.                                      | Dev / Prod                             |
| `php8.3-mbstring`  | Soporte para cadenas multibyte (UTF-8, UTF-16). Necesario para aplicaciones multilingües y manejo de formularios. | Dev / Prod                             |
| `php8.3-xml`       | Procesamiento de XML: DOM, SimpleXML, XMLReader/Writer. Necesario para APIs y configuración en XML.               | Dev / Prod                             |
| `php8.3-curl`      | Permite realizar peticiones HTTP desde PHP, esencial para consumir APIs externas.                                 | Dev / Prod                             |
| `php8.3-mysql`     | Conector MySQL/MariaDB para PHP (mysqli y PDO). Indispensable para bases de datos.                                | Dev / Prod                             |
| `php8.3-pdo`       | Interfaz de acceso a bases de datos de manera segura, soporta múltiples motores.                                  | Dev / Prod                             |
| `php8.3-gd`        | Manipulación de imágenes (crear, redimensionar, procesar PNG, JPEG).                                              | Dev / Prod                             |
| `php8.3-zip`       | Permite crear y extraer archivos ZIP.                                                                             | Dev / Prod                             |
| `php8.3-bcmath`    | Operaciones matemáticas de precisión arbitraria, útil para cálculos financieros.                                  | Dev / Prod                             |
| `php8.3-opcache`   | Cache de código PHP compilado para mejorar rendimiento. Normalmente activo en producción.                         | Prod                                   |
| `php8.3-soap`      | Permite consumir y crear servicios web SOAP.                                                                      | Dev / Prod si se usan APIs SOAP        |
| `php8.3-json`      | Soporte para codificación y decodificación de JSON. Esencial para APIs REST y almacenamiento de datos.            | Dev / Prod                             |
| `php8.3-xmlrpc`    | Comunicación vía XML-RPC con otros servicios web.                                                                 | Dev / Prod si se usan servicios legacy |
| `php8.3-gmp`       | Operaciones matemáticas con números grandes y criptografía.                                                       | Dev / Prod según necesidades           |
| `php8.3-memcached` | Conexión a sistemas de cache Memcached, mejora rendimiento de aplicaciones web.                                   | Prod                                   |
| `php8.3-redis`     | Conexión a Redis, útil para cache, sesiones y colas de trabajo.                                                   | Prod                                   |
| `php8.3-xdebug`    | Depuración avanzada, trazas y profiling de scripts. **Solo en desarrollo**.                                       | Dev                                    |
| `php8.3-ldap`      | Conexión con servidores LDAP (por ejemplo Active Directory).                                                      | Dev / Prod si se usan servicios LDAP   |
| `php8.3-mbstring`  | Soporte para cadenas multibyte. Indispensable para UTF-8.                                                         | Dev / Prod                             |

##### Como instalar módulos
```bash
sudo apt install "nombre del modulo"
sudo systemctl restart php-fpm
```
##### Verificación de módulos activos
```bash
php -m         # Lista todos los módulos instalados y activos
php -i | grep "modulo"  # Busca un módulo específico
```
#### Configuración PHP
En **/etc/php/8.3/apache2/php.ini** y **/etc/php/8.3/fpm/php.ini**

- Cambiar **display_errors** a "On"
- Cambiar **display_startup_errors** a "On"

![Alt](webroot/images/display_errors_php.png)
- Cambiar **memory_limit** a "256M"

![Alt](webroot/images/memory_limit_php.png)

En **/etc/php/8.3/apache2/php.ini** y **/etc/php/8.3/fpm/php.ini**

- Cambiar **date.timezone** a "Europe/Madrid"

![Alt](webroot/images/datetimezone1.png)

#### Entorno de desarrollo PHP
Activar errores:
En `/etc/php/8.3/fpm/php.ini` y `/etc/php/8.3/apache2/php.ini`:
```ini
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
log_errors = On
error_log = /var/log/php_errors.log
```

Registro de errores personalizado:
```bash
sudo touch /var/log/php_errors.log
sudo chown www-data:www-data /var/log/php_errors.log
sudo chmod 664 /var/log/php_errors.log
```

Recargar PHP-FPM para aplicar cambios:
```bash
sudo systemctl restart php8.3-fpm
sudo systemctl reload apache2
```
#### Entorno de explotación PHP
Configurar php.ini:
```ini
display_errors = Off
display_startup_errors = Off
log_errors = On
error_log = /var/log/php_errors.log
```

Nivel de registro recomendado:
```ini
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
```
#### Monitorización PHP
Ficheros log de PHP-FPM:

Error log de PHP-FPM: `/var/log/php8.3-fpm.log`
```bash
tail -f /var/log/php8.3-fpm.log
```

Registro de errores de scripts:

Cada script PHP puede generar errores en `/var/log/php_errors.log` definido en `php.ini`

Estado del servicio:
```bash
sudo systemctl status php8.3-fpm
sudo systemctl restart php8.3-fpm
```
#### Mantenimiento PHP
Reiniciar PHP-FPM:
```bash
sudo systemctl restart php8.3-fpm
```

Recargar configuración sin interrumpir conexiones:
```bash
sudo systemctl reload php8.3-fpm
```

Verificar módulos activos:
```bash
php -m
```

Backup de configuración:
```bash
sudo cp -r /etc/php/8.3 /etc/php/8.3_backup_$(date +%F)
```
---

### 1.2.3 MariaDB

#### Instalación
```bash
#Instalar MariaDB
sudo apt udpate
sudo apt install mariadb-server -y
```
#### Configuración
```bash
#Cambiar en el fichero indicado la linea bind-address = 127.0.0.1 (lo cambiamos a 0.0.0.0)
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
![Alt](webroot/images/mariadb1.png)
```bash
#Reiniciamos el servicio
sudo systemctl restart mariadb

#Comprobamos puerto usado por MariaDB (usará el 3306 por defecto)
sudo ss -punta |grep mariadb

#Lo habilitamos
sudo ufw allow 3306

#Entramos en la consola de MariaDB
sudo mariadb

#Creamos un nuevo usuario
CREATE USER 'adminsql'@'%' IDENTIFIED BY 'paso'
GRANT ALL ON *.* TO 'adminsql'@'%' WITH GRANT OPTION;

#Listar todos los usuarios y desde que host pueden conectarse
SELECT User, Host FROM mysql.user;

#Asegurar MariaDB ejecutando un sript de seguridad
sudo mysql_secure_installation

- En el primer paso preguntará por la contraseña de root para MariaDB, pulsa la tecla Enter ya que no hay contraseña definida.
- La siguiente, preguntará si quieres asignar una contraseña para el usuario “root”, le damos que si y le ponemos una.
- En el tercer paso preguntará si quieres eliminar usuario anónimo, aquí indica que Sí quieres borrar los datos.
- Después preguntará si quieres desactivar el acceso remoto del usuario “root”, aquí indica que Sí quieres desactivar acceso remoto para usuario por seguridad.
- De nuevo preguntará si quieres eliminar la base de datos test, aquí indica de nuevo que Sí quieres borrar las base de datos de prueba.
- Por último, preguntará si quieres recargar privilegios, aquí indica que Sí.
```
---

### 1.2.4 XDebug

#### Instalación
```bash
#Instalar
sudo apt install php8.3-xdebug
```
#### Configuración
```bash
#Entrar en /etc/php/8.3/fpm/conf.d/20-xdebug.ini y añadir
xdebug.mode=develop,debug
xdebug.start_with_request=yes
xdebug.client_host=127.0.0.1
xdebug.client_port=9003
xdebug.log=/tmp/xdebug.log
xdebug.log_level=7
xdebug.idekey="netbeans-xdebug"
xdebug.discover_client_host=1

#Reiniciar servidor
sudo systemctl restart php8.3-fpm
```

### 1.2.5 Apache HTTPS

![Alt](webroot/images/https0.png)
```bash
#Para crear certificado digital:
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/"nombre".key -out /etc/ssl/certs/"nombre".crt

luego nos pedirá varios datos, los ponemos y ya estaría creado
#Comprobar certificados en:
sudo ls /etc/ssl/private |grep "nombre"
sudo ls /etc/ssl/certs |grep "nombre"

#Habilitar módulo ssl
sudo a2enmod ssl

#Crear (o copiar) configuración del sitio seguro
sudo cp default-sss.conf "nombre".conf

#Cambiar rutas al certificado dentro de "nombre".conf donde he marcado en verde
```
![Alt](webroot/images/https1.png)

```bash
Crear configuración de sitio seguro:
Archivo: /etc/apache2/sites-available/mi_sitio.conf

<VirtualHost *:443>
    ServerAdmin webmaster@mi_sitio.com
    ServerName mi_sitio.com
    DocumentRoot /var/www/html

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/mi_sitio.crt
    SSLCertificateKeyFile /etc/ssl/private/mi_sitio.key

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error_ssl.log
    CustomLog ${APACHE_LOG_DIR}/access_ssl.log combined
</VirtualHost>


# Activar sitio y puerto HTTPS:

sudo ufw allow 443
sudo a2ensite mi_sitio.conf
sudo systemctl reload apache2


# Redirección HTTP a HTTPS:

<VirtualHost *:80>
    ServerName mi_sitio.com
    Redirect permanent / https://mi_sitio.com/
</VirtualHost>
```

### 1.2.6 DNS

##### Virtual Hosts
0. Previamente crear la regla DNS en explotación
1. Comprobar usuario enjaulado SFTP\
El sitio creado anteriormente va a ser de un usuario enjaulado (ver en el apartado de Enjaulado de usuarios)
2. Crear nuevo sitio.conf\
Copiamos el archivo de configuracion del sitio por defecto en un nuevo archivo ``nuevo-sitio``.conf y dentro lo tenemos que dejar así:\
![Alt](webroot/images/vh2.png)
Ahora metemos los archivos de nuestro sitio en httpdocs en la ruta indicada conectandonos con el usuarioenjaulado1 por sftp
- Si utiliza base de datos y tenemos los respectivos scripts de esa base de datos para crearla y darle carga inicial los ejecutamos con ``sudo mariadb -u adminsql -p < script.sql``\
Ahora si entramos en http://(ServerName del nuevo sitio) tiene que salirnos el index que subimos antes a httpdocs

### 1.2.7 SFTP

#### Enjaulado de usuarios
```bash
# Crear el grupo para meter a los usuarios enjaulados
sudo groupadd sftpusers
# Creación del usuario y cambio de contraseña
sudo useradd -g www-data -G sftpusers -m -d /var/www/nombredeusuario nombredeusuario

sudo passwd nombredeusuario
# El propietario del directorio jaula y los directorios sobre este, debe ser root. 
# El home del usuario pertenece al root 
sudo chown root:root /var/www/nombredeusuario
# Eliminar el permiso de escritura 
sudo chmod 555 /var/www/nombredeusuario
```
Por lo tanto, el usuario no tendría privilegios de escritura sobre su directorio. Para evitar ese problema se crea un directorio ‘ httpdocs ’, dentro de la jaula, que sea de propiedad y es allí donde él pueda escribir como leer archivos.
```bash
# Creacion de la carpeta httpdocs
sudo mkdir /var/www/nombredeusuario/httpdocs
# Permisos de httpdocs
sudo chmod2775 –R /var/www/nombredeusuario/httpdocs
# Propietarios de httpdocs
sudo chown nombredeusuario:www-data –R /var/www/nombredeusuario/httpdocs
```
Editar /etc/ssh/sshd_config
```bash 
# Subsystem sftp /usr/lib/openssh
Subsystem sftp internal
Match Group sftpusers 
ChrootDirectory %h
ForceCommand internal-sftp -u 2 
AllowTcpForwarding yes 
PermitTunnel no 
X11Forwarding no
```
---

### 1.2.8 Apache Tomcat

---

### 1.2.9 LDAP

---

## 2. Documentación de Código
**8. Como se hacen los comentarios para documentación PHPDoc**

Usar siempre 
```php
/** 
 * ...
*/
```

Documentar:

- Clases
- Métodos
- Controladores

Usar etiquetas estándar:

```php
@package
@author
@version
@param
@return
``` 
Ejemplo:

```php
/**
 * Controlador de Login
 *
 * @package Controladores
 * @version 1.0
 */
 ```

### 2.1 PHPDoc / phpDocumentor

**1. Requisitos Mínimos**

- **Sistema Operativo**: Ubuntu/Debian (o distribuciones basadas en APT)
- **PHP**: Versión 8.1 o superior (en este manual se usa PHP 8.3)
- **Extensiones PHP requeridas**:
  - php-xml (DOM, XMLWriter, SimpleXML)
  - php-mbstring
- **Herramientas del sistema**: wget, sudo
- **Espacio en disco**: Al menos 50 MB para phpDocumentor y las dependencias

**2. Verificar si las extensiones PHP ya están instaladas:**
```bash
php -m | grep -E "xml|mbstring"
```
Si aparecen `xml` y `mbstring` en la salida, las extensiones ya están instaladas y se puede omitir su instalación.


**3. Instalación**

Se actualiza el servidor
```bash
sudo apt update
sudo apt upgrade
```
Si no están instaladas se instalan las extensiones de PHP que se necesita para procesar archivos y plantillas XML/HTML.  
```bash
sudo apt install php8.3-xml
```
```bash
sudo apt install php8.3-mbstring
```

Reiniciar el servicio de PHP: Para que las extensiones recién instaladas se
carguen.
```bash
sudo service php8.3-fpm restart
```

Descarga e Instalación de phpDocumentor usando wget para descargar el ejecutable en el servidor  
```bash
wget https://phpdoc.org/phpDocumentor.phar
```
Se pasa el archivo al servidor y se le otorgan los permisos de ejecución
```bash
sudo chmod +x phpDocumentor.phar
```
Se mueve a una ubicación global /usr/local/bin y se renombra a phpdoc para poder ejecutarlo desde cualquier directorio
```bash
sudo mv phpDocumentor.phar /usr/local/bin/phpdoc
```
Se ejecuta phpdoc
```bash
phpdoc
```
Se entra en la carpeta del código fuente:
```bash
cd /var/www/html/CMVDWESAplicacionFinal
```
Para que no haya problema para que se cree la carpeta docs en la carpeta codigoPHP, hay que dar permisos.
```bash
sudo chmod -R 775 /var/www/html/CMVDWESAplicacionFinal
```

* Se ejecuta el phpDocumentor
```bash
phpdoc --directory . --target docs

# --directory .: Busca archivos PHP en el directorio actual.

# --target docs: Genera el HTML de salida en la carpeta docs.
```
---

> **Cristian Mateos Vega**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web 
