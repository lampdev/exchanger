FROM mysql:5.7

ARG SYSTEM_TZ
ARG MYSQL_ROOT_PASSWORD
ARG MYSQL_DATABASE
ARG MYSQL_USER
ARG MYSQL_PASSWORD

RUN ln -snf /usr/share/zoneinfo/$SYSTEM_TZ /etc/localtime && echo $SYSTEM_TZ > /etc/timezone

RUN set -x \
    && echo 'sql-mode="NO_ENGINE_SUBSTITUTION"' >> /etc/mysql/mysql.conf.d/mysqld.cnf \
    && echo 'datadir=/var/lib/mysql2' >> /etc/mysql/mysql.conf.d/mysqld.cnf

RUN printf '%s\n%s\n' "DROP DATABASE IF EXISTS ${MYSQL_DATABASE};CREATE DATABASE IF NOT EXISTS ${MYSQL_DATABASE};USE ${MYSQL_DATABASE};DROP USER '${MYSQL_USER}'@'%';FLUSH PRIVILEGES;CREATE USER '${MYSQL_USER}'@'%' IDENTIFIED BY '${MYSQL_PASSWORD}';GRANT ALL PRIVILEGES ON ${MYSQL_DATABASE}.* TO '${MYSQL_USER}'@'%'"

RUN sed -i '/exec "\$@"/c\#exec "\$@"' /entrypoint.sh
RUN /entrypoint.sh mysqld
