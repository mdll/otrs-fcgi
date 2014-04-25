Install otrs, running with nginx via fast-cgi
=============================================
For running otrs with nginx, you have to do some dependencies:

# 1. Required Software-Packages

this command will install all packages needed on Ubuntu/Debian:

	root@otrs:~$ apt-get install mysql-server nginx spawn-fcgi libnet-dns-perl libio-socket-ssl-perl \
	libnet-ldap-perl libgd-text-perl libgd-graph-perl libpdf-api2-perl libsoap-lite-perl libuser \
	libyaml-libyaml-perl libtext-csv-perl libtext-csv-xs-perl libfcgi-perl libmail-imapclient-perl \
	libjson-xs-perl libcrypt-eksblowfish-perl libencode-hanextra-perl

# 2. download otrs in current version

this command will download configure otrs on local system.

	root@otrs:~$ wget http://ftp.otrs.org/pub/otrs/otrs-3.x.x.tar.gz
	root@otrs:~$ cd /var/local/
	root@otrs:~$ tar -xzf otrs-3.x.x.tar.gz
	root@otrs:~$ ln -s otrs-3.x.x otrs
	root@otrs:~$ chmod -R www-data.www-data otrs*
