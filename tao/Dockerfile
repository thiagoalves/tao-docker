FROM php:7.0-apache

RUN apt-get update && apt-get install -y git libxml2-dev curl libcurl4-gnutls-dev sudo zlib1g-dev unzip && \
    docker-php-ext-install mbstring dom pdo pdo_mysql curl zip && \
    curl -s https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    mkdir /var/www/.composer && chown -R www-data:www-data /var/www/ && \
    sudo -u www-data git clone https://github.com/oat-sa/package-tao.git /var/www/html

WORKDIR /var/www/html

RUN sudo -u www-data composer install

RUN mkdir -p data && chown -R www-data:www-data . && chmod 777 data && sudo a2enmod rewrite

#ENV HOSTNAME localhost
ENV ADMIN_USERNAME admin
ENV ADMIN_PASSWORD admin

ENV DB_HOST mysql
ENV DB_NAME tao
ENV DB_USER root
ENV DB_PASSWORD tao

RUN echo sudo -u www-data php tao/scripts/taoInstall.php --db_driver pdo_mysql --db_host $DB_HOST \
      --db_name $DB_NAME --db_user $DB_USER --db_pass $DB_PASSWORD --module_namespace http://$HOSTNAME/first.rdf \
      --module_url http://$HOSTNAME --user_login $ADMIN_USERNAME --user_pass $ADMIN_PASSWORD -e taoCe

CMD sudo -u www-data php tao/scripts/taoInstall.php --db_driver pdo_mysql --db_host $DB_HOST \
      --db_name $DB_NAME --db_user $DB_USER --db_pass $DB_PASSWORD --module_namespace http://$HOSTNAME/first.rdf \
      --module_url http://$HOSTNAME --user_login $ADMIN_USERNAME --user_pass $ADMIN_PASSWORD -e taoCe && \
    apache2-foreground
#USER www-data
