FROM toixtran/ubuntu:16.04

MAINTAINER toi.xtran <toi.xtran@gmail.com>

ENV DEBIAN_FRONTEND="noninteractive" \
    MYSQL_ROOT_PASSWD="123456"

# Update & upgrade
RUN apt-get update && \
    apt-get -y upgrade

# Set root password for mysql
RUN echo "mysql-server mysql-server/root_password password ${MYSQL_ROOT_PASSWD}" | debconf-set-selections && \
    echo "mysql-server mysql-server/root_password_again password ${MYSQL_ROOT_PASSWD}" | debconf-set-selections

# Install common packages
RUN apt-get -y install \
    mysql-server-5.7 \
    mysql-client
# Config Mysql
RUN mkdir -p /var/lib/mysql && \
    mkdir -p /var/run/mysqld && \
    mkdir -p /var/log/mysql && \
    chown -R mysql:mysql /var/lib/mysql && \
    chown -R mysql:mysql /var/run/mysqld && \
    chown -R mysql:mysql /var/log/mysql

# UTF-8 and bind-address
RUN sed -i -e "$ a [client]\n\n[mysql]\n\n[mysqld]"  /etc/mysql/my.cnf && \
    sed -i -e "s/\(\[client\]\)/\1\ndefault-character-set = utf8/g" /etc/mysql/my.cnf && \
    sed -i -e "s/\(\[mysql\]\)/\1\ndefault-character-set = utf8/g" /etc/mysql/my.cnf && \
    sed -i -e "s/\(\[mysqld\]\)/\1\ninit_connect='SET NAMES utf8'\ncharacter-set-server = utf8\ncollation-server=utf8_unicode_ci\nbind-address = 0.0.0.0/g" /etc/mysql/my.cnf

ADD /root/etc/supervisord.d/mysql.ini /etc/supervisord.d/mysql.ini

VOLUME /var/lib/mysql

EXPOSE 3306

CMD ["/usr/bin/supervisord", "--configuration=/etc/supervisord.conf"]
