FROM occitech/magento:php5.5-apache

ENV MAGENTO_VERSION 1.9.2.4

RUN cd /tmp && \
    curl https://codeload.github.com/OpenMage/magento-mirror/tar.gz/$MAGENTO_VERSION -o $MAGENTO_VERSION.tar.gz && \
    tar xvf $MAGENTO_VERSION.tar.gz && \
    mv magento-mirror-$MAGENTO_VERSION/* magento-mirror-$MAGENTO_VERSION/.htaccess /var/www/htdocs && \
    chown -R www-data:www-data /var/www/htdocs && \
    chmod -R g+rw /var/www/htdocs

RUN apt-get update && \
    apt-get install -y mysql-client-5.5 libxml2-dev && \
    docker-php-ext-install soap

COPY ./bin/install-magento /usr/local/bin/install-magento
RUN chmod +x /usr/local/bin/install-magento

COPY ./sampledata/magento-sample-data-1.9.1.0.tgz /opt/
COPY ./bin/install-sampledata-1.9 /usr/local/bin/install-sampledata
RUN chmod +x /usr/local/bin/install-sampledata

RUN bash -c 'bash < <(curl -s -L https://raw.github.com/colinmollenhour/modman/master/modman-installer)' && \
    mv ~/bin/modman /usr/local/bin && \
    a2enmod ssl

VOLUME /var/www/htdocs
RUN sed -i 's/DocumentRoot \/var\/www\/html/DocumentRoot \/var\/www\/htdocs/' /etc/apache2/sites-available/000-default.conf && \
    sed -i 's/DocumentRoot \/var\/www\/html/DocumentRoot \/var\/www\/htdocs/' /etc/apache2/sites-available/default-ssl.conf
