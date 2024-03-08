# Instalación de una pila LAMP en Ubuntu Server
## Creación de una máquina instancia EC2 en AWS
Para crear nuestra instancia EC2, vamos a Amazon EC2 en la consola AWS. Hacemos click al botón naranja **"Lanzar Instancia"** para abrir el asistente de creación de asistencias.

![Imagen 1] (images/Screenshot 2023-10-19 160014.png)

### 1. Elegimos una AMI (Amazon Machine Image)


![Imagen Paso 1](https://user-images.githubusercontent.com/114667075/235878557-2ffcf791-f178-4454-82c0-80fd72081fe6.png)

### Paso 2. Elija un tipo de instancia

En la segunda pantalla del asistente de EC2, seleccionamos un tipo de instancia EC2. Un tipo de instancia es una configuración particular de CPU, memoria (RAM), almacenamiento y capacidad de red.

AWS tiene una amplísima selección de tipos de instancias que abarca diferentes tipos de cargas de trabajo. 

![Imagen Paso 2](https://user-images.githubusercontent.com/114667075/235849812-924b7e24-6a1d-4b0d-b7d4-bd304f019bd1.png)

**⚠️IMPORTANTE!!!⚠️**

Tenemos que tener en cuenta en el apartado de "Configuración de red" el marcar las casillas para permitir el tráfico en HTTP y HTTPS ya que serán necesarias en la práctica.

![Configuración de red](https://user-images.githubusercontent.com/114667075/235878884-f34e1c61-2e62-41dc-9c6d-1cdbe9a6efa9.png)

Ya realizada toda la configuración que tendrá nuestra instancia solo nos faltará dar clic al botón naranja **"Lanzar instancia"**.

![Lanzar instancia](https://user-images.githubusercontent.com/114667075/235879113-3bd66e1a-9913-408f-bbbf-74bbdf75861b.png)

### Paso 3. IP elástica

Ya creada la instancia, la página nos asignara una IP por defecto que continuamente va cambiando debido a la escasez de IPs (ya que estas no son ilimitadas). Asignaremos este tipo de IP a nuestra instancia para asi tener una IP estática que no va cambiando diariamente que es diseñada en la nube dinámica.

Para acceder a este servicio nos iremos a :
- Servicios (Arriba a la izquierda del inicio de AWS)
  - Red y seguridad
    - Direcciones IP elástica

Empezaremos la creación de nuestra nueva IP elástica dandole clic a **"Asignar la dirección IP elástica"**.

!["Asignar la dirección IP elástica"](https://user-images.githubusercontent.com/114667075/235883328-c6b553fb-fa17-479d-b08b-b2224c362f47.png)

Al darle clic nos aparecera una ventana donde podremos seleccionar el grupo fronterizo de red, direcciones ip elásticas globales y etiquetas. Cuando realizamos la configuración que mas se adapte a nestras necesidas daremos clic a **"Asignar"**.

!["Asignar"](https://user-images.githubusercontent.com/114667075/235884032-54baf32b-179c-41df-a48c-9e5fef8bc1a4.png)

Para asignar esta IP elástica desde el mismo servicio seleccionaremos nuestra IP elástica y le daremos clic a **"Acciones > Asociar la dirección IP elástica"**.

![Donde asociar la ip a la instancia](https://user-images.githubusercontent.com/114667075/235886439-4fea714e-1730-4c89-aabe-d905a64ab9fd.png)

Luego de darle clic, nos saldra una ventana donde seleccionaremos la instancia a la que queremos asignar la dirección de IP elástica y le daremos clic a **"Asociar"**.

![Asociamos la IP elástica](https://user-images.githubusercontent.com/114667075/235887078-f69e9065-7b45-4d90-96a5-68e93a1ed87c.png)

### Paso 3.  Lanzar nuestra instancia y obtener una clave SSH

Para lanzar nuestra instancia es tan sencillo como seleccionar aquella instancia que queramos lanzar y damos clic a **"Lanzar instancia"**.

![Lanzar instancia](https://user-images.githubusercontent.com/114667075/235887904-8d123641-bd74-4292-b644-97b619d0a0c8.png)

Para obtener esta clave lo que haremos es dar clic a :

- Panel de control
  - Seleccionamos nuestro curso
    - Contenidos
      - Laboratorio para el alumnado
        - AWS Details
          - Dowload PEM

![Dowload PEM](https://user-images.githubusercontent.com/114667075/235889667-fd1110cb-7fbd-48fb-bc9c-7f9131a99b7a.png)

Ya obtenido el .pem lo único que tendremos que hacer es cambiar los permisos de este para poder usarlo con total libertad y el nombre a **"vockey.pem"**. El motivo del cambio del nombre del archivo es debido a que es nuestras instancias son asignadas estos nombres en espeícifco para poder conectarse por ssh.

En el caso de **Windows**, para poder cambiar los permisos de tu usuario daremos clic derecho a la clave y nos iremos a :
- Propiedades
  - Seguridad
    - Opciones avanzadas
      - Seleccionamos nuestro usuario o grupos de usuario al que queramos aplicar los cambios dando doble clic izquierdo
        - Y en permisos básicos marcamos la casilla de "Control total"

![Screenshot 2023-05-09 084030](https://user-images.githubusercontent.com/114667075/237015298-733a6d5c-5f71-4f8e-aff1-55ec025a19ae.jpg)

Ya solo nos queda conectarnos a nuestra instancia por ssh desde Visual Studio Code (También tienes la opción de acceder a la instancia desde un terminal dentro de la página web).

Antes de empezar la conexion por ssh nos tendremos que instalaar una extensión en nuestro Visual Studio Code llamada **"Remote - SSH"** y de forma complementaria su otra extensión **"Remote - SSH: Editing Configuration Files"**.

![Screenshot 2023-05-09 085636](https://user-images.githubusercontent.com/114667075/237018215-53c6668f-c682-424a-b3fb-7cd94f86a947.png)

Instaladas las extensiones procederemos a seguir los siguientes pasos:

1. En el lado izquierdo nos aparecera un nuevo icono con forma de monitor en el que daremos clic izquierdo para abrir el menu  de ssh

2. Dentro del menú, nos saldra una carpeta o contenedor llamada **"SSH"** en el que pasaremos el raton por encima y nos aparecera un engranaje donde daremos clic izquierdo para continuar con la conexión. Al dar clic, nos daran a elegir para acceder a dos directorios distintos ( en nuestro caso daremos clic al directorio de ".../config"

3. Ya accediendo al archivo config tendremos que poner las credenciales correspondientes como la IP o el nombre de usuario.

![Screenshot 2023-05-09 084941](https://user-images.githubusercontent.com/114667075/237020844-d95ceeb6-80a1-4616-ac6e-c050db56b931.jpg)

Ahora solo nos queda conectarnos a nuestra instanca por ssh dando clic derecho a nuestra instancia y elegir la conexion en la ventana actual o abrir una nueva ventana.

![Screenshot 2023-05-09 090956](https://user-images.githubusercontent.com/114667075/237021979-f49ab02d-a124-47c6-a5e6-38ed1d60c0fb.png)

## Instalación de la pila LAMP en la instancia

Lo primero de todo será realizar una carpeta en nuestra máquina para guardar todos nuestros scripts y poder localizarlos facilmente.

Ya creada la carpeta nos tocará realizar nuestro archivo .sh donde realizaremos la instalación de nuestra pila LAMP ( en nuestro caso lo hemos llamado "install_lamp.sh")

**⚠️IMPORTANTE!!!⚠️**

A la hora de realizar todos nuestros scripts es importante escribir estos dos comandos:

`
#!/bin/bash
`

Con este comando indicamos al sistema operativo que invoque el shell especificado para ejecutar los comandos que siguen en el script.

`
set -x
`

Habilitamos para que se muestre los comandos que se van ejecutando.

Ya aprendido todo esto, podemos continuar con la creación de nuestro script.

### 1. Actualizamos los repositorios.

`
apt update
`

### 2. Actualizamos paquetes.

`
apt upgrade -y
`

### 3. Instalamos el servidor web Apache.

`
apt install apache2 -y
`

### 4. Instalamos el servidor MYSQL.

`
apt install mysql-server -y
`

### 5.1. Instalamos los módulos de PHP necsarios.

`
apt install php libapache2-mod-php php-mysql -y
`

### 5.2. Reiniciamos el servidor web Apache.

`
systemctl restart apache2
`

### 5.3. Copiamos el archivo  de prueba de PHP al directorio de Apache.

`
cp ../php/index.php /var/www/html
`

### 5.4. Copiamos el archivo dir.conf al directorio de configuración de Apache.

`
cp ../conf/dir.conf /etc/apache2/mods-available
`

### 6.  Reiniciamos el servidor web Apache.

`
systemctl restart apache2
`

Ya realizado esste script, nos tocará realizar otro archivo .sh donde realizaremos instalaciones de ciertas herramientas para nuestra pila LAMP y su debida configuración.
Antes de empezar con el script, tendremos que crear un par de carpetas dentro de nuestra práctica ("conf" y "php") donde guardaremos ciertos datos y configuraciones de las herramientas instaladas a continuación.

### 1.1. Instalación de las dependencias.

`
apt install php-mbstring php-zip php-gd php-json php-curl -y
`

### 1.2. Descargamos el código fuente desde la página oficial.

`
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.1/phpMyAdmin-5.2.1-all-languages.zip -O /tmp/phpmyadmin.zip
`

### 1.3. Instalamos la utilidad unzip.

`
apt install unzip -y
`

### 1.4. Descomprimimos el código fuente de phpMyAdmin.

`
unzip -o /tmp/phpmyadmin.zip -d /var/www/html
`

### 1.5. Eliminamos las instalaciones previas de phpMyAdmin.

`
rm -rf /var/www/html/phpmyadmin
`

### 1.6. Renombramos el directorio de phpMyAdmin.

`
mv /var/www/html/phpMyAdmin-5.2.1-all-languages /var/www/html/phpmyadmin
`

### 1.7. Creamos el archivo de configuración a partir del archivo de ejemplo.

`
mv /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/config.inc.php
`

### 1.8. Creamos la base de datos de phpMyAdmin.

`
mysql -u root < /var/www/html/phpmyadmin/sql/create_tables.sql
`

###  1.9. Creamos un usuario en MySQL para phpMyAdmin.

`
mysql -u root <<< "DROP USER IF EXISTS $PHPMYADMIN_USER@localhost;"
`

`
mysql -u root <<< "CREATE USER $PHPMYADMIN_USER@localhost IDENTIFIED BY '$PHPMYADMIN_PASS';"
`

`
mysql -u root <<< "GRANT ALL PRIVILEGES ON phpmyadmin.* TO $PHPMYADMIN_USER@localhost;"
`

Estas variables que veis en el comando serán dadas y escritas dentro del script. Se recomienda poner estas variables al principio para que sea fácil identificarlas y poder cambiarlas rapidamente.

`
Variables de configuración
`

`
#-------------------------------------------------------------------------------
`

`
PHPMYADMIN_USER=phpmyadmin_user
`

`
PHPMYADMIN_PASS=phpmyadmin_pass
`

`
#-------------------------------------------------------------------------------
`

### 1.10. Modificamos el propietario y el grupo.

`
chown -R www-data:www-data /var/www/html
`








