FROM ubuntu:bionic 
EXPOSE 80
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Madrid
RUN apt update && \
apt upgrade -y && \
apt install -y apache2 php libapache2-mod-php && \
apt install -y php-xml php-curl php-gd php-zip 
COPY web /var/www/html/ 
COPY 000-default.conf /etc/apache2/sites-enabled/ 
RUN chown -R www-data:www-data /var/www/html/
RUN a2enmod rewrite
RUN service apache2 restart
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
