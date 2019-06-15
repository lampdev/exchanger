FROM baseimage-php

WORKDIR /var/www/html

COPY composer.json ./
COPY composer.lock ./
RUN composer install --no-scripts --no-autoloader

COPY . ./
RUN composer install --no-scripts

RUN chown -R www-data var

RUN /bin/echo -e '\
ErrorLog "/dev/stderr"\n\
CustomLog "/dev/stdout" common\n\
<VirtualHost *:80>\n\
    DocumentRoot "/var/www/html/public"\n\
    <Directory "/var/www/html/public">\n\
        AllowOverride None\n\
        Order Allow,Deny\n\
        Allow from All\n\
        <IfModule mod_rewrite.c>\n\
            Options -MultiViews\n\
	        Options FollowSymlinks\n\
            RewriteEngine On\n\
            RewriteCond %{REQUEST_FILENAME} !-f\n\
            RewriteRule ^(.*)$ index.php [QSA,L]\n\
        </IfModule>\n\
    </Directory>\n\
</VirtualHost>\n\
' > /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite

RUN /bin/echo -e "#!/bin/bash\n\
php /var/www/html/bin/phpunit\n\
php /var/www/html/bin/console --no-interaction doctrine:migrations:migrate\n\
php /var/www/html/bin/console app:update-rates\n\
chmod +x /var/www/html/cron/command.sh\n\
crontab /var/www/html/cron/cron.d\n\
cron\n\
chmod -R 0777 /var/www/html/var/cache\n\
chmod -R 0777 /var/www/html/var/log\n\
apache2 -v\n\
exec apachectl -D FOREGROUND\n\
" > /usr/sbin/docker-entrypoint.sh \
    && chmod +x /usr/sbin/docker-entrypoint.sh


CMD /usr/sbin/docker-entrypoint.sh
