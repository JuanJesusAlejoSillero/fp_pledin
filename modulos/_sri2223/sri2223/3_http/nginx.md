---
title: "Introducción al servidor web nginx"
---

## Instalación de nginx

	apt-get update
	apt-get install nginx

## Introducción a virtual hosting con nginx

Podemos encontrar los sitios disponibles y activos del servidor nginx, siguiendo la filosofía de Apache2 en los directorios:

	/etc/nginx/sites-availables/
	/etc/nginx/sites-enabled/

Para activar un nuevo sitio tenemos que crear el enlace directo en el directorio `/etc/nginx/sites-enabled/`:

	ln -s /etc/nginx/sites-available/test.com /etc/nginx/sites-enabled/

En el fichero `/etc/nginx/sites-availables/default` nos encontramos la directiva:

	listen 80 default_server;

`default_server;` nos permite indicar el sitio virtual por defecto.

## Migrando configuración de Apache2 a nginx

Ejemplo de configuración básdica de un sitio virtual en apache2:

	<VirtualHost *:80>
		ServerName www.example.com
		ServerAdmin webmaster@localhost
		DocumentRoot /var/www/html

        Alias /prueba/ /var/www/html/prueba
	    <Directory /var/www/html/prueba>
	        Options Indexes FollowSymLinks
	        AllowOverride None
	        Require all granted
	    </Directory>

	    Alias /doc/ "/usr/share/doc/"
    	<Directory "/usr/share/doc/">
    	    Options Indexes MultiViews FollowSymLinks
    	    AllowOverride None
    	    require ip 127.0.0.0/255.0.0.0 ::1/128
    	</Directory>

		ErrorLog ${APACHE_LOG_DIR}/error.log
		CustomLog ${APACHE_LOG_DIR}/access.log combined

	</VirtualHost>

Si traducimos la misma configuración en un servidor nginx:

	server {
	    listen 80;

	    root /var/www/html;
	    index index.html index.htm;

	    server_name www.example.com;

	    location / {
	        try_files $uri $uri/ /index.html;
	    }

		location /prueba/ {
	        alias /var/www/html/prueba;
	        autoindex on;
	        allow all;
	    }

	    location /doc/ {
	        alias /usr/share/doc/;
	        autoindex on;
	        allow 127.0.0.1;
	        deny all;
	    }
	}

Podríamos resumir las correspondencias en el siguiente cuadro:

|Apache                                     |Nginx                           
|-------------------------------------------|-----------------------------------
|`<VirtualHost *:80>`                       | `server {` <br/>`       listen 80;`
|`ServerName yoursite.com`	        	    | `server_name www.yoursite.com;`  
|`DocumentRoot /path/to/root`               |`root /path/to/root;`
|`AllowOverride All`                        |No podemos usar .htaccess
|`DirectoryIndex index.php`                  |`index index.php;`
|`ErrorLog /path/to/log`                      |`error_log /path/to/log error;`
|`CustomLog /path/to/log combined`            |`access_log /path/to/log main;`
|`Alias /url/ "/path/to/files"`<br/>`<Directory "/path/to/files">`|`location /url/ {`<br/>`     alias /path/to/files;`<br/>`}`
|`Options Indexes`                            |`autoindex on`
|`Require all granted`                        |`allow all`
|`allow 127.0.0.1`                            |`allow 127.0.0.1;`
|`deny all`                                   |`deny all;`
|`proxy_pass / http://localhost:8080` <br/>`proxy_pass_reverse / http://localhost:8080 `|`location / {`<br/>`    proxy_pass http://localhost:8080;`<br/>`    proxy_set_header X-Forwarded-Host $host:$server_port;`<br/>`    proxy_set_header X-Forwarded-Server $host;`<br/>`    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`<br/>`}`



