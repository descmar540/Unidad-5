# Unidad-5LM

El repositorio está enfocado a la totalidad de los archivos subidos de la Unidad 5, pero ahora voy a explicar los pasos para poner en marcha GetSimpleCMS utilizando Docker en Ubuntu.

## PASOS A SEGUIR ##

1. Lo primero que tenemos que hacer es crear la carpeta "webphp".

2. Creamos el archivo Dockerfile mediante el comando "nano Dockerfile", y pegamos esta información. Guardamos y salimos.
~~~
FROM ubuntu:bionic  
EXPOSE 80  
ENV DEBIAN_FRONTEND=noninteractive  
ENV TZ=Europe/Madrid  
RUN apt update && \  
apt upgrade -y && \  
apt install -y apache2 php libapache2-mod-php && \  
apt install -y php-xml php-curl php-gd php-zip  
COPY web /var/www/html/  
RUN chown -R www-data:www-data /var/www/html/  
RUN a2enmod rewrite  
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]  
~~~

3. Creamos el archivo 000-default.conf mediante el comando "nano 000-default.conf", y como hacíamos en el paso, pegamos la siguiente información. Guardamos y salimos.
~~~
<virtualhost *:80>       
# The ServerName directive sets the request scheme, hostname and port that
# the server uses to identify itself. This is used when creating
      
# redirection URLs. In the context of virtual hosts, the ServerName
# specifies what hostname must appear in the request's Host: header to
# match this virtual host. For the default virtual host (this file) this
# value is not decisive as it is used as a last resort host regardless.
# However, you must
# set it for any further virtual host explicitly.
# ServerName www.example.com

ServerAdmin webmaster@localhost
      
DocumentRoot /var/www/html
# Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
# error, crit, alert, emerg.
        
# It is also possible to configure the loglevel for particular
 # modules, e.g.
 # LogLevel info ssl:warn
 <Directory /var/www/html/simplecms/>
       Options Indexes FollowSymLinks MultiViews
       AllowOverride all
       Order allow,deny
       allow from all
 </Directory>
 ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
      
# For most configuration files from conf-available/, which are
# enabled or disabled at a global level, it is possible to
# include a line for only one
# particular virtual host. For example the
# following line enables the CGI configuration for this host only
# after it has been globally disabled with "a2disconf".
       
#Include conf-available/serve-cgi-bin.conf
</VirtualHost>
~~~
Con esto, habilitamos el **mod_rewrite** para que funcione y podamos establecer el valor **AllowOverride** *al*en la carpeta donde estamos utilizando el GetSimpleCMS. 

4. Creamos la carpeta "web" dentro de "webphp", accedemos a ella y descargamos el programa GetSimpleCMS desde su página mediante el comando "wget http://get-simple.info/latest".

5. Descomprimimos el archivo haciendo unzip, y lo renombramos para que que se llame **"simplecms"**

6. El siguiente paso sería ejecutar el Docker build, para lo cual, obviamente, necesitaremos tener Docker instalado. Para ello, podemos ejecutar el build mediante el comando *"docker build -t simplecms ."*

7. Si todo ha salido bien (sin que nos dé ningún error), el siguiente paso es ejecutar el Docker mediante un Docker run. Utilizaremos este comando *"docker run -d --rm -p 80:80 -v simplecms_data:/var/www/html/simplecms simplecms"*.

Ya lo tendremos pues todo listo. Si todo ha ido bien, introduciendo la ip pública de nuestro Ubuntu en el navegador seguido de /simplecms, debería de aparecer nuestra página de GetSimpleCMS lista para verse, y si además añadimos /admin, nos dejará modificarla desde la propia web, donde podremos añadir más páginas, modificar las existentes, etc.
* Para acceder a nuestra página: IP + /simple. Ejemplo: **http://192.168.0.1/simplecms**
* Para modificar y administrar nuestra página: **http://192.168.0.1/simplecms/admin/**


