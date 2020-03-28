FROM ubuntu:16.04
RUN apt-get update -y 
RUN  apt-get install apache2 apache2-utils -y 
EXPOSE 80
RUN apt-get install php7.0 php7.0-mysql libapache2-mod-php7.0 php7.0-cli php7.0-cgi php7.0-gd -y
COPY info.php /var/www/html/info.php
ADD wordpress /var/www/wordpress
RUN rm -rvf /var/www/html && mv /var/www/wordpress /var/www/html
RUN chown -R www-data /var/www/html && chmod -R 755 /var/www/html
RUN cd /var/www/html && mv wp-config-sample.php wp-config.php
RUN sed -i "s@define( 'DB_NAME', 'database_name_here' )@define( 'DB_NAME', 'wordpress' )@g" /var/www/html/wp-config.php
RUN sed -i "s@define( 'DB_USER', 'username_here' )@define( 'DB_USER', 'admin' )@g" /var/www/html/wp-config.php
RUN sed -i "s@define( 'DB_PASSWORD', 'password_here' )@define( 'DB_PASSWORD', 'password' )@g" /var/www/html/wp-config.php
RUN sed -i "s@define( 'DB_HOST', 'localhost' )@define( 'DB_HOST', 'mysql' )@g" /var/www/html/wp-config.php
CMD ["apachectl", "-D", "FOREGROUND"]
#systemctl enable apache2 && systemctl start apache2
