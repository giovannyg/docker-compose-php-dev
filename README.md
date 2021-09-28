# docker-compose-lamp
A docker compose configuration to build php web apps

## Images: ##

- Apache 
- MySQL
- PostgreSQL
- Php
- Artisan (basically same as php, but with an entrypoint)
- Composer
- Npm & node

It has two different docker-compose files basically with distinct database configurations (MySQL and PostgreSQL), rename one of those files to ```docker-compose.yml``` depending on which database you're gonna use.

As this configuration has entrypoints for composer, npm and artisan to easily run commands from host machine using docker-compose, i.e:

```
docker-compose run composer install
docker-compose artisan make:controller
docker-compose npm install
docker-compose npm run watch
```
