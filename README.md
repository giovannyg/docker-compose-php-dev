# docker-compose-lamp
A docker compose configuration to build php web apps

## Images: ##

- Apache/Nginx
- php
- artisan
- composer
- node

As this configuration has entrypoints for composer, npm and artisan you can easily run commands from host machine using docker-compose, i.e:

```
docker-compose run php composer install
docker-compose run artisan make:controller
docker-compose run npm install
docker-compose run npm run watch
```

Optionally you can add a script called `dcr` (with execute permission) in a directory present on your `PATH` (for instance `~/.local/bin/`) with this content:

```
#!/bin/bash

if [[ $# -eq 0 ]]; then
  echo 'No se ha recibido ningun argumento'
  exit 0
fi

FILE=./bin/dcr.sh
if [[ ! -f "$FILE" ]]; then
  echo "no existe el archivo $FILE"
  exit 0
fi

./bin/dcr.sh $@
```

and now you'll be able to run commands like this:

```
dcr artisan --version
```
