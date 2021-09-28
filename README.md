# docker-compose-lamp
A lamp stack development environment using docker + docker-compose

** Alpine images **

## Services ##

* Apache 
	- httpd:2.4.35-alpine
	- Includes a httpd-vhosts file
	- DocumentRoot: public_html
* MySQL
	- mysql:8.0.13
* Php
	- php:7.3-rc-fpm-alpine
