<!-- TITLE: Instalacion Server Debian -->
<!-- SUBTITLE: Proceso de instalación servidor Debian -->

# APR
```sh
cd /usr/local/src/
wget http://mirrors.axint.net/apache//apr/apr-1.4.6.tar.gz
tar -xvzf apr-1.4.6.tar.gz
cd apr-1.4.6/
./configure
make
make install
```
# APR Utils
```sh
wget http://mirrors.axint.net/apache//apr/apr-util-1.4.1.tar.gz
tar -xvzf apr-util-1.4.1.tar.gz
cd apr-util-1.4.1
./configure --with-apr=/usr/local/apr
make
make install
cd..
```
# PCRE
```sh
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.41.tar.gz
tar -xvzf pcre-8.41.tar.gz
cd pcre-8.41
./configure --prefix=/usr/local/pcre
make
make install
```
# ZLIB
```sh
wget http://prdownloads.sourceforge.net/libpng/zlib-1.2.11.tar.gz
tar -xvzf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make
make install
cd ..
```
# OPENSSL
```sh
wget https://www.openssl.org/source/old/1.0.2/openssl-1.0.2.tar.gz
tar -xvzf openssl-1.0.2.tar.gz
cd openssl-1.0.2
./config shared --prefix=/usr
make
make install
cd ..
```
# APACHE
```sh
wget https://archive.apache.org/dist/httpd/httpd-2.4.25.tar.gz
tar -xvzf httpd-2.4.25.tar.gz
cd httpd-2.4.25
./configure --prefix=/usr/local/apache --enable-file-cache --enable-cache --enable-disk-cache -enable-ssl --enable-mem-cache --enable-deflate --enable-expires --enable-headers --enable-usertrack --enable-ssl --enable-cgi --enable-vhost-alias --enable-rewrite --enable-so --with-apr=/usr/local/apr/ -with-ssl=/usr/include/openssl/ --with-pcre=/usr/local/pcre --enable-proxy=shared
make
make install
```
Iniciar el servicio
```sh
/usr/local/apache/bin/apachectl start
```
# JPGE Support
```sh
wget http://www.ijg.org/files/jpegsrc.v9b.tar.gz
tar xzvf jpegsrc.v9b.tar.gz
./configure -enable-shared
make
make install
cp libjpeg.* /usr/lib/
```
# libxml++
```sh
apt-get install libxml++2.6-dev
```
# GD
```sh
wget https://www.pccc.com/downloads/gd/gd-2.0.33.tar.gz
tar xzvf gd-2.0.33.tar.gz
cd gd-2.0.33
./configure 
make
make install
```
# libmycrypt
```sh
apt-get install libmcrypt-dev
```
# PNG.h
```sh
apt-get install libpng-dev
```
# libbz2
```sh
apt-get install libbz2-dev
```
# PHP
```sh
wget http://br2.php.net/distributions/php-5.6.30.tar.gz
tar xzvf php-5.6.30.tar.gz
cd php-5.6.30
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/lib --with-zlib-dir --enable-zip --with-bz2 --with-openssl --with-apxs2=/usr/local/apache/bin/apxs --enable-mbstring --with-mcrypt --with-gd --with-jpeg-dir=/usr/lib
make
make install
cp php-5.6.30/php.ini-production /usr/local/php/lib/php.ini
nano /usr/local/apache/conf/httpd.conf
```
Buscar la linea
```sh
LoadModule php5_module        modules/libphp5.so
```
y adicionar enseguida
```sh
<IfModule dir_module>
        DirectoryIndex index.html index.php
</IfModule>

AddHandler php5-script .php
AddType application/x-httpd-php .php .phtml
AddType application/x-httpd-php-source .phps
```
Luego
```sh
nano /etc/profile.d/php.sh
```
Adicionar las siguientes lineas en el archivo
```sh
PATH=$PATH:/usr/local/php/bin
export PATH
```
Guardar y salir, luego crear el archivo
```sh
nano /etc/profile.d/php.csh
```
Y adicionar las variables
```sh
PATH=$PATH:/usr/local/php/bin
export PATH
```
Editar el archivo .ini
```sh
nano /usr/local/php/lib/php.ini
```
buscar date.timezone y dejar como 
```sh
date.timezone = America/Bogota
```
exportar variable php

crear o editar el archivo /root/.bash_profile con la informacion
```sh
PATH=$PATH:/usr/local/php/bin/
export PATH
```

Reiniciar el servicio
```sh
/usr/local/apache/bin/apachectl restart
```

Finalmente reiniciar la terminal
# MySql
Instalación
```sh
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.17-linux-glibc2.5-x86_64.tar.gz
tar zxvf mysql-5.7.17-linux-glibc2.5-x86_64.tar.gz

groupadd mysql
useradd -r -g mysql -s /bin/false mysql
cd /usr/local
ln -s /usr/local/src/mysql-5.7.17-linux-glibc2.5-x86_64 mysql
cd mysql
mkdir mysql-files
chmod 750 mysql-files
chown -R mysql .
chgrp -R mysql .

chown -R root .
chown -R mysql data
#chmod 750 data

scripts/mysql_install_db --user=mysql# MySQL 5.7.0 to 5.7.4
bin/mysql_install_db --user=mysql    # MySQL 5.7.5
bin/mysqld --initialize --user=mysql # MySQL 5.7.6 and up
bin/mysql_ssl_rsa_setup              # MySQL 5.7.6 and up

chown -R root .
chown -R mysql data mysql-files
bin/mysqld_safe --user=mysql &
```
#Next command is optional
```sh
cp support-files/mysql.server /etc/init.d/mysql.server
systemctl start mysqld
```
Postinstalación
```sh
/usr/local/mysql/bin/mysql_install_db --user=mysql --basedir=/usr/local/mysql --datadir=/var/lib/mysql/data/
```
#varialbles de ambiente
```sh
nano /etc/profile.d/mysql.sh
PATH=$PATH:/usr/local/mysql/bin
export PATH

nano /etc/profile.d/mysql.csh
PATH=$PATH:/usr/local/mysql/bin
export PATH
```
#al reiniciar la máquina ya estarán disponibles estos comandos
```sh
/usr/local/mysql/bin/mysqladmin -u root password 'password' 
```
# PHPMYADMIN
```sh
wget https://files.phpmyadmin.net/phpMyAdmin/4.6.6/phpMyAdmin-4.6.6-all-languages.tar.gz
tar zxvf phpMyAdmin-4.6.6-all-languages.tar.gz
mv phpMyAdmin-4.6.6-all-languages /usr/local/apache/htdocs/phpMyAdmin
useradd phpmy
passwd phpmy
cd /usr/local/apache/htdocs
chown -R phpmy.daemon phpMyAdmin/
```
#Recompilar php con mysql
```sh
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/lib --with-zlib-dir --enable-zip --with-bz2 --with-openssl --with-apxs2=/usr/local/apache/bin/apxs --enable-mbstring --with-mcrypt --with-gd --with-jpeg-dir=/usr/lib --with-mysql --with-mysqli 
```
Reiniciar apache
```sh
/usr/local/apache/bin/apachecrt restart
```
# libreadline-dev
```sh
apt-get install libreadline-dev
```
# POSTGRES 
```sh
wget https://ftp.postgresql.org/pub/source/v9.6.2/postgresql-9.6.2.tar.gz
tar zxvf postgresql-9.6.2.tar.gz
cd postgresql-9.6.2
./configure —prefix=/usr/local/pgsql --datadir=/var/lib/pgsql
make
make install

adduser postgres
chown postgres /var/lib/pgsql
su - postgres
/usr/local/pgsql/bin/initdb -D /var/lib/pgsql/data
#/usr/local/pgsql/bin/postmaster -D /var/lib/pgsql/data >logfile 2>&1 &
```
#Iniciar el servicio 
```sh
/usr/local/pgsql/bin/pg_ctl -D /var/lib/pgsql/data -l logfile start
exit
```
verificar que exitan los archivos de configuracion 
```sh
ls -lh /var/lib/pgsql/data 
```
si están con extensión .sample cambiarlos por .conf
```sh
nano /etc/profile.d/postgresql.sh

PGSQL_HOME=”/usr/local/pgsql”
#PGDATA=$PGHOME/data
PGDATA=/var/lib/pgsql/data
PATH=/usr/local/pgsql/bin:$PATH
export PATH

nano /etc/profile.d/postgresql.csh

PGSQL_HOME=”/usr/local/pgsql”
#PGDATA=$PGHOME/data
PGDATA=/var/lib/pgsql/data
PATH=/usr/local/pgsql/bin:$PATH
export PATH
cp /usr/local/src/postgresql-9.6.2/contrib/start-scripts/linux /etc/init.d/postgresql
chmod +x /etc/init.d/postgresql

su - postgres
createuser tuproyecto
psql template1
ALTER USER tuproyecto WITH ENCRYPTED PASSWORD 'password';
\q
```
#Restaurar base de datos http://www.damianculotta.com.ar/2008/10/14/restaurar-backups-de-postgresql/
```sh
pg_restore -h localhost -p 5432 -U tuproyecto -d tuproyecto -v '/home/admin/bd_postgres/tuproyectodb151218-1.backup'

apt-get install curl
apt-get install libcurl4-gnutls-dev
```
## Recompile php
```sh
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/lib --with-zlib-dir --enable-zip --with-bz2 --with-openssl --with-apxs2=/usr/local/apache/bin/apxs --enable-mbstring --with-gd --with-jpeg-dir=/usr/lib --with-mysqli --with-pdo-mysql --enable-pdo --enable-cli --with-pgsql=/usr/local/pgsql --with-pdo-pgsql=/usr/local/pgsql --with-curl --enable-calendar --enable-soap
```
## Compilar Contrib
```sh
cd /usr/local/src/postgresql-9.6.2/contrib/
make
make install
```
Ingresar con el usuario postgres y crear el tipo de campo
```sh
su - postgres
psql template1
CREATE EXTENSION IF NOT EXISTS citext WITH SCHEMA public;
\dx
```
# PHPPGADMIN
```sh
wget http://downloads.sourceforge.net/phppgadmin/phpPgAdmin-5.1.tar.gz
tar zxvf phpPgAdmin-5.1.tar.gz
nano phpPgAdmin/conf/config.inc.php
```
# Git
```sh
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```
# NODE
```sh
wget https://nodejs.org/dist/v8.7.0/node-v8.7.0.tar.gz
tar zxvf node-v8.7.0.tar.gz
./configure
make
make install
```

Iniciar aplicación Node JS

sobre scripst/node
```sh
pm2 start ecosystem.config.js --env production
```
# Configuracion SSL
```sh
sudo apt-get update
sudo apt-get upgrade openssl
```
Enable the Apache SSL module
```sh
sudo a2enmod ssl
sudo a2ensite default-ssl
sudo service apache2 reload
```
Crear la carpeta para los certificados
```sh
sudo mkdir /usr/local/apache/ssl
```
Generar los certificados
```sh
openssl genrsa -out coposoftware.key 2048
openssl req -new -sha256 -days 365 -newkey rsa:2048 -key /usr/local/apache/ssl/coposoftware.key -out /usr/local/apache/ssl/coposoftware.csr
```
Activar en apache los modulos de ssl
```sh
LoadModule socache_shmcb_module modules/mod_socache_shmcb.so
LoadModule ssl_module modules/mod_ssl.so
Include conf/extra/httpd-ssl.conf
```