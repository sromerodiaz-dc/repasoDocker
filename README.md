# repasoDocker

En primer lugar descargamos la imagen de alpine con el siguiente comando.

    ```docker pull alpine```

Comprobamos que la imagen se ha descargado correctamente con el siguiente comando.

    ```docker images```

    ### 2. Crea un contenedor sin ponerle nombre. ¿está arrancado? Obtén el nombre
Creamos un contenedor sin nombre con el siguiente comando.

    ```docker run -it alpine```

Comprobamos que el contenedor se ha creado correctamente con el siguiente comando.

    ```docker ps -a```

Obtenemos el nombre del contenedor con el siguiente comando.

    ```docker ps -a --format "{{.Names}}"```
    ### 3. Crea un contenedor con el nombre 'dam_alp1'. ¿Como puedes acceder a él?
Creamos un contenedor con el nombre 'dam_alp1' con el siguiente comando.

    ```docker run -it --name dam_alp1 alpine```

Accedemos al contenedor con el siguiente comando.

    ```docker exec -it dam_alp1 /bin/sh```

    ### 5. Crea un contenedor con el nombre 'dam_alp2'. ¿Puedes hacer ping entre los contenedores?
Creamos un contenedor con el nombre 'dam_alp2' con el siguiente comando.

    ```docker run -it --name dam_alp2 alpine```

Comprobamos la ip del contenedor con el siguiente comando.

    ```docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' dam_alp2```

Hacemos ping al contenedor 'dam_alp1' con el siguiente comando.

    ```docker exec -it dam_alp2 ping
    ### 6. Sal del terminal, ¿que ocurrió con el contenedor?

Salimos del terminal con el comando 'exit'. El contenedor sigue en ejecución.

    ```docker ps -a```

### 7. ¿Cuanta memoria en el disco duro ocupaste?

Comprobamos la memoria ocupada en el disco duro con el siguiente comando.

    ```docker system df```

  ### 8. ¿Cuanta RAM ocupan los contenedores? ¿Hay algún comando docker para saber esto?

Comprobamos la RAM ocupada por los contenedores con el siguiente comando.

    ```docker stats``` 
1. ### Descarga la imagen 'httpd' y comprueba que está en tu equipo.
    Descargamos la imagen de apache2 con el siguiente comando:
    ```docker pull httpd:2.4```
    Comprobamos que la imagen se ha descargado correctamente con el siguiente comando:
    ```docker images```

   2. ### Crea un contenedor con el nombre 'dam_web1'.

    Creamos el contenedor con el siguiente comando:
    ```docker run -d --name dam_web1 httpd:2.4```
    Comprobamos que el contenedor se ha creado correctamente con el siguiente comando:
    ```docker ps```

   3. ### Si quieres poder acceder desde el navegador de tu equipo, ¿que debes hacer?
    Primero debemos parar y eliminar el contenedor, para poder crear otro con el puerto mapeado con el siguiente comando:
    
    ```docker stop dam_web1``` y ```docker rm dam_web1```

    Para poder acceder desde el navegador de mi equipo, debo mapear el puerto del contenedor al puerto de mi equipo con el siguiente comando:

    ```docker run -d --name dam_web1 -p 8000:80 httpd:2.4```

    Comprobamos que el contenedor se ha creado correctamente con el siguiente comando:

    ```docker ps```

   4. ### Utiliza bind mount para que el directorio del apache2 'htdocs' esté montado un directorio que tu elijas.

    Creamos un directorio en nuestro equipo con el siguiente comando:

    ```mkdir /home/anxo/damWeb```

    Creamos un contenedor con el directorio mapeado con el siguiente comando:

    ```docker run -d --name dam_web1 -p 8000:80 -v /home/anxo/damWeb:/usr/local/apache2/htdocs httpd:2.4```

    Comprobamos que el contenedor se ha creado correctamente con el siguiente comando:

    ```docker ps```

   5. ### Realiza un 'hola mundo' en html y comprueba que accedes desde el navegador.
    Creamos un archivo index.html en el directorio /home/anxo/damWeb con el siguiente contenido:

        <html>
            <head>
                <title>Hola Mundo</title>
            </head>
            <body>
                <h1>Hola Mundo</h1>
            </body>
        </html>

   6. ### Crea otro contenedor 'dam_web2' con el mismo bind mount y a otro puerto, por ejemplo 9080.

    Creamos un contenedor con el directorio mapeado y en otro puerto con el siguiente comando:

    ```docker run -d --name dam_web2 -p 9080:80 -v /home/anxo/damWeb:/usr/local/apache2/htdocs httpd:2.4```

    Comprobamos que el contenedor se ha creado correctamente con el siguiente comando:

    ```docker ps```

   ### 1. Utiliza la imagen de Ubuntu , tag 22 y apoyandote en esta guía sigue sus instrucciones para instalar LAMP en dicho contenedor.

En primer lugar vamos a descargar la imagen de Ubuntu 22.04 utilizando el siguiente comando:

    docker pull ubuntu:22.04

Una vez descargada la imagen, vamos a crear un contenedor con la misma utilizando el siguiente comando:

    docker container create -i -t -p 8000:80 --name ubuntu22.04 ubuntu:22.04

Una vez creado el contenedor lo arrancamos con el siguiente comando:

    docker start --attach -i  ubuntu22.04 

   Una vez dentro vamos a instalar la pila LAMP siguiendo los siguientes pasos:
1. Actualizamos los paquetes del sistema:

        apt update

2. Instalamos Apache:

        apt install -y apache2 apache2-utils

3. Instalamos mariadb:

        apt install -y mariadb-server mariadb-client

4. Iniciamos maria db, y configuramos la contraseña de root:

        service mariadb start
        mysql_secure_installation
      
   Aqui nos pedira la contraseña de root, la cual dejaremos en blanco, y luego nos pedira que la cambiemos, la cambiamos y le damos a todo a "Y" para que configure el resto de opciones.


5. Instalamos PHP:
    
         apt install -y php php-mysql libapache2-mod-php

6. Probamos que funciona:

    echo \"\<?php phpinfo(); ?>\" | tee /var/www/html/info.php

7. Iniciamos apache:

        service apache2 start

   Accedemos al navegador con la sigueinte ruta:

   http://10.0.9.188:8000/info.php 
    ### 2. Utiliza esta guía para instalar wordpress en el contenedor.

Para instalar wordpress en el contenedor, vamos a seguir los siguientes pasos:

1. Instalamos las dependencias: (ALgunas de ellas ya estan instaladas, pero como no supone un problema copiaremos el scrip directamente de la guia)

        apt update
         apt install apache2 \
         ghostscript \
         libapache2-mod-php \
         mysql-server \
         php \
         php-bcmath \
         php-curl \
         php-imagick \
         php-intl \
         php-json \
         php-mbstring \
         php-mysql \
         php-xml \
         php-zip
2. Instalamos wordpress:

         mkdir -p /srv/www
         chown www-data: /srv/www
         curl -o https://wordpress.org/latest.tar.gz
         tar zx -C /srv/www -f latest.tar.gz

3. Configuramos apache para WordPress:

         touch /etc/apache2/sites-available/wordpress.conf
         nano /etc/apache2/sites-available/wordpress.conf

   Dentro del archivo pegamos el siguiente contenido:

        <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /srv/www/wordpress
            ServerName wordpress.local
            ServerAlias www.wordpress.local
            <Directory /srv/www/wordpress>
                Options FollowSymLinks
                AllowOverride All
                Require all granted
            </Directory>
            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>

   Habilita el sitio con:

            a2ensite wordpress
            service apache2 reload

   Habilita la reescritura de URL con:

       a2enmod rewrite

   Deshabilita el sitio predeterminado "It Works" con:

         a2dissite 000-default

   Reinicia Apache con:

         service apache2 restart

4. Configuramos la base de datos (POR UN ERROR TUVE QUE REINSTALAR MARIADB EN ESTE PASO):

         mysql -u root -p
         CREATE DATABASE wordpress;
         CREATE USER 'anxo' IDENTIFIED BY '1234';
         GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER ON wordpress.* TO 'anxo';
         FLUSH PRIVILEGES;
         QUIT;

5. Configuramos WordPress:

   Accedemos desde el navegador a http://10.0.9.188:8000/

   6. wp-config:
   
   Creamos el archivo de configuración de WordPress:

         touch /srv/www/wordpress/wp-config.php
         nano /srv/www/wordpress/wp-config.php

    Dentro del archivo pegamos el siguiente contenido:

  ```` 
  <?php
/**
   * The base configuration for WordPress
     *
     * The wp-config.php creation script uses this file during the installation.
     * You don't have to use the website, you can copy this file to "wp-config.php"
     * and fill in the values.
     *
     * This file contains the following configurations:
     *
 * * Database settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
   *
     * @link https://developer.wordpress.org/advanced-administration/wordpress/wp-config/
     *
     * @package WordPress
     */

// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'anxo' );

/** Database password */
define( 'DB_PASSWORD', '1234' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8mb4' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/**#@+
 * Authentication unique keys and salts.
   *
   * Change these to different unique phrases! You can generate these using
   * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
   *
   * You can change these at any point in time to invalidate all existing cookies.
   * This will force all users to have to log in again.
   *
   * @since 2.6.0
   */
define( 'AUTH_KEY',         's[u1wXGyctU,mlo?e]@9oF8H5 2TvN{cXIW$Bka$2y1mHiewl^#wbs?5^w}bqiZV' );
define( 'SECURE_AUTH_KEY',  '%c|p_{W18!)`0Z-;$o[0_Gw!>FcjP,lRJXi/M1[?#>o0:p[@su$]P=DIR}$BqjG(' );
define( 'LOGGED_IN_KEY',    'G.ui.lm42ILdmyeWLg)~:rO,6o`gkUeDKq9]$7x$snYHW6!I0>A3cRK~!x ]` z<' );
define( 'NONCE_KEY',        '[xUHnT-um:]H;X._l7p_#J!T3M`Iy]Od%%.*ImS~2_Z{?s)2t). 6GQie0,;Y!n4' );
define( 'AUTH_SALT',        '_p7zJt7xK1O6Ap{&J,a#2{eFlCLKi*ZMw3A!1bR#WT?@srOmM$e);X<+ia]<`b?a' );
define( 'SECURE_AUTH_SALT', '%3{{P[$)R5dLwk=uE3NcPo]`;f`ND5oO!@E.cc9`.X/Qgz719lt.0uP:UuB56CKu' );
define( 'LOGGED_IN_SALT',   '8x&Xtei=>P#7NI@f,yGOS7-9V}LMKS/-X^UiVLzKlV8GQBv8%I[#f7^nP^~N/7N3' );
define( 'NONCE_SALT',       'OLK3Lg|^p|&<p~{{B8@g%t/a}wXpV(R(HETQ7Y1o}YiW&@[a#%4Gs1|b*j?RoZ1.' );

/**#@-*/

/**
 * WordPress database table prefix.
   *
   * You can have multiple installations in one database if you give each
   * a unique prefix. Only numbers, letters, and underscores please!
   */
$table_prefix = 'wp_';

/**
 * For developers: WordPress debugging mode.
   *
   * Change this to true to enable the display of notices during development.
   * It is strongly recommended that plugin and theme developers use WP_DEBUG
   * in their development environments.
   *
   * For information on other constants that can be used for debugging,
   * visit the documentation.
   *
   * @link https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/
   */
define( 'WP_DEBUG', false );

/* Add any custom values between this line and the "stop editing" line. */



/* That's all, stop editing! Happy publishing. */

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
	define( 'ABSPATH', __DIR__ . '/' );
}

/** Sets up WordPress vars and included files. */
require_once ABSPATH . 'wp-settings.php';
````

### 1. Crea un archivo docker-compose.yml con la configuración necesaria para poner en marcha Prestashop.

Para hacerlo de manera mas organizada primero creo una carpeta llamada prestashop y me situo en ella:

```mkdir prestashop```

```cd prestashop```

Creamos el archivo docker-compose.yml:

```touch docker-compose.yml```

Accedemos a el con nano:

```nano docker-compose.yml```

Y añadimos el siguiente contenido:

```
services:
  mysql:
    container_name: some-mysql
    image: mysql:5.7
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: admin
      MYSQL_DATABASE: prestashop
    networks:
      - prestashop_network
  prestashop:
    container_name: prestashop
    image: prestashop/prestashop:latest # Latest stable version of the PrestaShop, to see all available images go to ...
    restart: unless-stopped
    depends_on:
      - mysql
    ports:
      - 8080:80
    environment:
      DB_SERVER: some-mysql
      DB_NAME: prestashop
      DB_USER: root
      DB_PASSWD: admin
    networks:
      - prestashop_network
networks:
    prestashop_network:
```

### 2. Ponemos a funcionar el contenedor con el siguiente comando:

```docker-compose up -d```

Podemos comprobar que esta todo correcto con:

```docker ps -a```
## Ejercicio: 

### 1 - Crear un archivo docker-compose.yml

````
services:
    Joomla:
        image: joomla
        restart: always
        ports:
            - 8080:80
        environment:
            JOOMLA_DB_HOST: db
            JOOMLA_DB_USER: joomla
            JOOMLA_DB_PASSWORD: 1234
            JOOMLA_DB_NAME: joomla_db
            JOOMLA_SITE_NAME: Joomla
            JOOMLA_ADMIN_USER: Joomla
            JOOMLA_ADMIN_USERNAME: joomla
            JOOMLA_ADMIN_PASSWORD: joomla@secured
            JOOMLA_ADMIN_EMAIL: joomla@example.com
    
    db:
        image: mysql:8.0
        restart: always
        environment:
            MYSQL_DATABASE: joomla_db
            MYSQL_USER: joomla
            MYSQL_PASSWORD: 1234
            MYSQL_RANDOM_ROOT_PASSWORD: '1234'
        
    phpMyAdmin:
        image: phpmyadmin
        restart: always
        ports:
            - 8081:80
        environment:
            - PMA_ARBITRARY = 1
````

### 2 - Iniciamos el docker-compose

```` docker compose up -d ````

### 3 - Comprobamos que los servicios estén corriendo

```` docker ps ````
