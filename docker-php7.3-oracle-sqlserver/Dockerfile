FROM debian:stretch

ADD files /tmp/files

RUN apt update -y \
&& apt upgrade -y \
&& apt install -y wget \
&& apt -y install lsb-release apt-transport-https ca-certificates  \
&& wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
&& echo "deb https://packages.sury.org/php/ stretch main" | tee /etc/apt/sources.list.d/php7.3.list \
&& apt update -y \
&& apt upgrade -y \
&& apt-get install -y apache2 \
&& apt-get install -y php7.3 \
&& apt-get install -y unixodbc-dev php7.3-dev php-pear \
&& apt-get install -y php7.3-soap php7.3-gd nano curl \
&& apt-get install -y zip  \
&& apt-get install -y php7.3-mysql php7.3-curl php7.3-json php7.3-cgi php7.3 php7.3-mbstring libapache2-mod-php7.3 php7.3-ldap php-xdebug wget \
&& apt-get install -y build-essential libaio1  \
&& apt-get install -y alien  \
&& cd /tmp/files \
&& unzip oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.zip \
&& unzip oracle-instantclient12.2-devel-12.2.0.1.0-1.x86_64.zip \
&& cd /tmp/files && alien oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.rpm \
&& cd /tmp/files && alien oracle-instantclient12.2-devel-12.2.0.1.0-1.x86_64.rpm \
&& cd /tmp/files && dpkg -i oracle-instantclient12.2-basic_12.2.0.1.0-2_amd64.deb \
&& cd /tmp/files && dpkg -i oracle-instantclient12.2-devel_12.2.0.1.0-2_amd64.deb \
&& printf "\n" | pecl install oci8 \
&& echo "extension=oci8.so" >> /etc/php/7.3/mods-available/oci8.ini \
&& cd /etc/php/7.3/apache2/conf.d && ln -s /etc/php/7.3/mods-available/oci8.ini oci8.ini \
&& cd /etc/php/7.3/cli/conf.d && ln -s /etc/php/7.3/mods-available/oci8.ini oci8.ini \
&& mkdir -p /usr/share/oracle/12.2/client64/network/admin \
&& echo " " >> /usr/share/oracle/12.2/client64/network/admin/tnsnames.ora \
&& echo " " >> /usr/share/oracle/12.2/client64/network/admin/sqlnet.ora \
&& export ORACLE_HOME=/usr/lib/oracle/12.2/client64 \
&& export TNS_ADMIN=/usr/share/oracle/12.2/client64/network/admin \
&& export LD_LIBRARY_PATH=/usr/lib/oracle/12.2/client64/lib/ \
&& rm -rf /tmp/files  \
&& apt-get update && apt-get install -y dpkg-dev \
&& apt-get install dirmngr --install-recommends \
&& chmod 777 /var/cache/apt/archives/partial/  \
&& chown _apt /var/cache/apt/archives/partial  \
&& mkdir /opt/php7 && chown -R _apt /opt/php7/ && cd /opt/php7  \
&& echo "deb-src ftp://ftp.de.debian.org/debian/ stable main contrib" > /etc/apt/sources.list \
&& apt-get update \
&& apt-get install xz-utils && cd /opt/php7/ && wget http://www.php.net/distributions/php-7.3.2.tar.xz && tar -Jxf php-7.3.2.tar.xz  \
&& export ORACLE_HOME=/usr/lib/oracle/12.2/client64 && cd /opt/php7/php-7.3.2/ext/pdo_oci && phpize && ./configure \
&& cd /opt/php7/php-7.3.2/ext/pdo_oci \
&& cp /usr/include/oracle/12.2/client64/oci.h include/oci.h \
&& cp /usr/include/oracle/12.2/client64/oratypes.h include/oratypes.h \
&& cp /usr/include/oracle/12.2/client64/ocidfn.h include/ocidfn.h \
&& cp /usr/include/oracle/12.2/client64/oci1.h include/oci1.h \
&& cp /usr/include/oracle/12.2/client64/oro.h include/oro.h \
&& cp /usr/include/oracle/12.2/client64/ori.h include/ori.h \
&& cp /usr/include/oracle/12.2/client64/ort.h include/ort.h \
&& cp /usr/include/oracle/12.2/client64/orl.h include/orl.h \
&& cp /usr/include/oracle/12.2/client64/ociextp.h include/ociextp.h \
&& cp /usr/include/oracle/12.2/client64/ociapr.h include/ociapr.h \
&& cp /usr/include/oracle/12.2/client64/ociap.h include/ociap.h \
&& cp /usr/include/oracle/12.2/client64/nzt.h include/nzt.h \
&& cp /usr/include/oracle/12.2/client64/nzerror.h include/nzerror.h \
&& cp /usr/include/oracle/12.2/client64/ocixmldb.h include/ocixmldb.h \
&& cp /usr/include/oracle/12.2/client64/oci8dp.h include/oci8dp.h \
&& cp /usr/include/oracle/12.2/client64/ocixstream.h include/ocixstream.h \
&& make && printf "\n" | make test  \
&& cp /opt/php7/php-7.3.2/ext/pdo_oci/modules/pdo_oci.so /usr/lib/php/20180731/  \
&& echo "; configuration for php pdo_oci module" > /etc/php/7.3/mods-available/pdo_oci.ini  \
&& echo "extension=pdo_oci.so" >> /etc/php/7.3/mods-available/pdo_oci.ini  \
&& phpenmod pdo_oci && a2enmod rewrite  \
&& wget https://phar.phpunit.de/phpunit.phar && chmod +x phpunit.phar && mv phpunit.phar /usr/local/bin/phpunit  \
&& export LD_LIBRARY_PATH=/usr/lib/oracle/12.2/client64/lib \
&& export LD_LIBRARY_PATH64=/usr/lib/oracle/12.2/client64/lib \
&& curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
&& echo "deb [arch=amd64] http://packages.microsoft.com/debian/9/prod/ stretch main" > /etc/apt/sources.list.d/mssql-release.list

RUN apt-get update -y
RUN ACCEPT_EULA=Y apt-get install msodbcsql17 mssql-tools -y
RUN apt-get update -y

RUN pecl install sqlsrv
RUN pecl install pdo_sqlsrv

RUN apt-get -y update
RUN apt-get -y install php7.3-zip
RUN phpenmod zip

RUN echo "extension=sqlsrv.so" > /etc/php/7.3/mods-available/sqlsrv.ini
RUN echo "extension=pdo_sqlsrv.so" > /etc/php/7.3/mods-available/pdo_sqlsrv.ini

RUN echo "127.0.0.1 localhost" > /etc/hosts
RUN echo "localhost localhost" > /etc/hosts
RUN echo "127.0.0.1 localhost" > /etc/hostname

RUN echo "ServerName localhost" > /etc/apache2/apache2.conf

RUN a2enmod rewrite
RUN phpenmod sqlsrv
RUN phpenmod pdo_sqlsrv


ENTRYPOINT [bash, -c, "service apache2 restart && tail -f /dev/null"]
