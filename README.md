Install otrs, running with nginx via fast-cgi
=============================================
For running otrs with nginx, you have to do some dependencies:

### 1. Required Software-Packages

this command will install all packages needed on Ubuntu/Debian:

	root@otrs:~$ apt-get install mysql-server nginx spawn-fcgi libnet-dns-perl libio-socket-ssl-perl \
	libnet-ldap-perl libgd-text-perl libgd-graph-perl libpdf-api2-perl libsoap-lite-perl libuser \
	libyaml-libyaml-perl libtext-csv-perl libtext-csv-xs-perl libfcgi-perl libmail-imapclient-perl \
	libjson-xs-perl libcrypt-eksblowfish-perl libencode-hanextra-perl git-common

### 2. Download otrs in current version

this command will download configure otrs on local system. this sample will set following local settings:

- HOMEDIR of otrs is /var/local/otrs (dynamic to version)
- USER starting fast-cgi (per default, web-user)

*Remember:* all calls for Ubuntu/Debian.

	root@otrs:~$ wget http://ftp.otrs.org/pub/otrs/otrs-3.x.x.tar.gz
	root@otrs:~$ cd /var/local/
	root@otrs:~$ tar -xzf otrs-3.x.x.tar.gz
	root@otrs:~$ ln -s otrs-3.x.x otrs
	root@otrs:~$ cp otrs/Kernel/Config.pm.dist otrs/Kernel/Config.pm
	root@otrs:~$ chmod -R www-data.www-data otrs*

### 3. Install scripts for this package

now its time to copy the scripts from this package to have a working fast-cgi. The files need some little
configuration, like HOMEDIR and USER...

	root@otrs:~$ mkdir -p /var/run/otrs
	root@otrs:~$ chown -R www-data.www-data /var/run/otrs
	root@otrs:~$ cd ~
	root@otrs:~$ git clone git@hub.mdll.de:secure/otrs-fcgi.git
	root@otrs:~$ cp otrs-fcgi/bin/fastcgi-wrapper.pl /usr/local/bin/
	root@otrs:~$ chmod +x /usr/local/bin/fastcgi-wrapper.pl
	root@otrs:~$ cp otrs-fcgi/init.d/otrs-fcgi /etc/init.d/
	root@otrs:~$ chmod +x /etc/init.d/otrs-fcgi
	
### 4. Configure webserver nginx

let's configure the webserver for starting the application. before copy nginx-configuration please check the
params...


	root@otrs:~$ cd ~
	root@otrs:~$ cp otrs-fcgi/nginx/otrs /etc/nginx/conf.d/otrs.conf
	root@otrs:~$ service nginx configtest
	root@otrs:~$ service nginx restart
	
### 5. Finally start the fast-cgi

with the following commands, you can start, stop and restart the fast-cgi:

start:
``
service otrs-fcgi start
``

stop:
``
service otrs-fcgi stop
``

restart:
``
service otrs-fcgi restart
``

the following command add the start to autostart

	root@otrs:~$ update-rc.d otrs-fcgi defaults
	root@otrs:~$ insserv otrs-fcgi
