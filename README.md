
# Códigos del tutorial de Wordpress
Producción II

 - [Da click aquí para ver el video
   tutorial](https://web.microsoftstream.com/video/d8489782-d8a5-4d8a-a811-9da4ac624531)

## Suplanta las palabras en mayúsculas con tu info

**Este es para obtener las listas de ubicaciones de servidores que tiene Azure**

    az account list-locations

**Con este creas un grupo de recursos**
**Deja celtralus**

    az group create --name NOMBRE_GRUPO_DE_RECURSOS --location centralus

**Para crear una maquina virtual en tu grupo de recursos**

    az vm create \
        --resource-group NOMBRE_GRUPO_DE_RECURSOS \
        --location centralus \
        --name NOMBRE_MAQUINA_VIRTUAL \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys

**Copia aquí tu IP**

    52.176.42.14
    DIRECCIÓN IP PÚBLICA

**Con este comando obtienes todas las direcciones IP de todas las maquinas virtuales de tu grupo de recursos**

    az network public-ip list --resource-group NOMBRE_GRUPO_DE_RECURSOS --query [].ipAddress

**Con este comando abres el puerto 80 que es necesario para que se mustre la página**

    az vm open-port --port 80 --resource-group NOMBRE_GRUPO_DE_RECURSOS --name NOMBRE_MAQUINA_VIRTUAL
    
    
**Es importante que en este apartado se haga la conexión en el grupo de recursos de red para poder visualizar la página en línea**

    Para esto te debes dirigir al grupo de recursos en tu portal de azure, identificar el grupo de recursos que se creó al principio y entrar ahí.
    Una vez que se accede, identifica el recurso de la máquina virtual y das click.
    En las pestañas de la parte superior del recurso en el apartado de información general, identifica la pestaña que dice "conectar" y selecciona SSH
    luego indica el SSH que se usa para poner la ip pública "azureuser@TU_DIRECCION_IP" en la parte de Ruta de acceso de clave privada.
    Copia el código que se genera en el punto 4 y ejecutalo en el bash y de esta forma será accesible desde la segunda dirección ip que te denera
    ssh -i <ruta de acceso de clave privada> azureuser@<ruta de acceso para el navegador>

**Con este comando accedes a la máquina virtual**

    ssh azureuser@TU_DIRECCION_IP

**Con este comando compruebas que tu maquina funciona y hace moo**
sudo apt-get moo

**Aquí actualizas los paquetes de linux (Como el sistema operativo) e instalas LAMP**

    sudo apt update && sudo apt install lamp-server^

L -> Linux: El sistema operativo de la maquina virtual
A -> Apache: El servidor que permite visualizar páginas en una maquina virtual
M -> MySQL: El sistema de base de datos que usará Wordpress
P -> PHP: El lenguaje de programación que usa y en el que está constuido Wordpress

**Te da la versión de Apache y verifica si está bien instalado**

    apache2 -v

**Te da la versión de MySQL y verifica si está bien instalado**

    mysql -V

**Te da la versión de PHP y verifica si está bien instalado**

    php -v

**Instalación de usuarios de MySQL y determinación de la contraseña**

    sudo mysql_secure_installation

**Acceder a MySQL**

    sudo mysql -u root -p

**Obtener la lista de los usuarios de MySQL**

    SELECT user FROM mysql.user;

**Salir de MySQL**

    exit;

**Crear el archivo info.php que te permite ver la versión de PHP y probar si todo ha salido bien hasta ahora**

    sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'

**Para probar si todo esta bien y visualizar el archivo inho.php que está en tu maquina virtual**

 - http://TU_DIRECCION_IP/info.php

**Instalar Wordpress**

    sudo apt install wordpress

**Para crear un archivo con los comandos necesarios para que Wordpress use la Base de Datos**

    sudo sensible-editor wordpress.sql

**Creas una base de datos y le das permisos**
**Solo cambia la contraseña, lo demás déjalo igual**

    CREATE DATABASE wordpress;
    GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
    ON wordpress.*
    TO wordpress@localhost
    IDENTIFIED BY 'TU_CONTRASEÑA_MYSQL';

**Copias el contenido del archivo anterior a la configuración de mysql para que lo use tu página**

    cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf

**Por seguridad, eliminas el archivo con tu contraseña antes creado**

    sudo rm wordpress.sql

**Es para el archivo de configuración necesario para Wordpress**

    sudo sensible-editor /etc/wordpress/config-localhost.php

**Para darle permisos a Wordpress de Acceder a la base de datos**
**Solo cambia la contraseña, lo demás déjalo igual**

    <?php
    define('DB_NAME', 'wordpress');
    define('DB_USER', 'wordpress');
    define('DB_PASSWORD', 'TU_CONTRASEÑA_MYSQL');
    define('DB_HOST', 'localhost');
    define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
    ?>

**Es para hacer un enlace simbólico y que la página "esté" también en el puerto 80 que es donde se ve las páginas**

    sudo ln -s /usr/share/wordpress /var/www/html/wordpress

**Para enviar toda la configuración de Wordpress al lugar que le corresponde y pueda funcionar la página**

    sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php

**Comprueba que tu página de Wordpress funciona**

 1. http://TU_DIRECCION_IP/wordpress

**Ir a la carpeta de Wordpress**

    cd /var/www/html/wordpress

**Muestra todo el contenido de la carpeta actual**

    ls

**Acceder al super usuario y tener todos los permisos**

    sudo su

**De aquí en adelante tienes que ir a GitHub, hacer un repositorio vacío y seguir los pasos que ahí te dan**

 - https://github.com

**IMPORTANTE**
Agrega: 

    git add .

Antes de hacer: 

git commit -m "Initial commit"


Si tienes problemas, este es el link para reportar errores de azure. 
No aplican errores por mal uso de la plataforma o errores humanos

 - https://azure.microsoft.com/en-us/support/create-ticket/
