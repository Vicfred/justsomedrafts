# Instalacion

Instalamos los paquetes necesarios, primero nginx

```
# emerge -av www-servers/nginx
```
Para uwsgi que es un servidor hecho en C para aplicaciones en python (y otros) como no vamos a tener aplicaciones en Ruby o PHP vamos a quitarlos:

en `/etc/portage/package.use/uwsgi` agregamos:

```
www-servers/uwsgi PYTHON_TARGETS: -* python3_7 RUBY_TARGETS: -* PHP_TARGETS: -*
```

Y luego lo instalamos

```
# emerge -av www-servers/uwsgi
```

Y creamos un usuario

```
# useradd -r -M -G uwsgi uwsgi
```


# Configuracion de nginx
Por defecto tenemos una configuracion de nginx que se encuentra en `/etc/nginx/nginx.conf` y la vamos a dejar asi pero podemos cambiarla un poco, por ejemplo, el numero de worker_processes

```
user nginx nginx;

worker_processes 4;
worker_rlimit_nofile 64000;

error_log /var/log/nginx/error_log info;

events {
        worker_connections 16000;
        multi_accept on;
        use epoll;
}

http {
      	log_format main
                '$remote_addr - $remote_user [$time_local] '
                '"$request" $status $bytes_sent '
                '"$http_referer" "$http_user_agent" '
                '"$gzip_ratio"';

        access_log off;

        disable_symlinks if_not_owner;
        ignore_invalid_headers on;
        server_tokens off;

        keepalive_timeout 20;
        client_header_timeout 20;
        client_body_timeout 20;
        reset_timedout_connection on;
        send_timeout 20;

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;
        charset UTF-8;

        gzip on;
        gzip_vary on;
        gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript text/x-js image/x-icon image/bmp;

        include /etc/nginx/sites-enabled/*;
}
```

Creamos si no existen dos directorios para la configuracion de sitios individuales

```
mkdir /etc/nginx/sites-available
mkdir /etc/nginx/sites-enabled
```

Y creamos una configuracion para un sitio en `/etc/nginx/sites-available/cyberia.moe.conf` para que nginx redireccione a uwsgi:

```
server {
        listen 80;
        server_name cyberia.moe www.cyberia.moe;
        error_log /var/log/nginx/cyberia.moe.error.log;
        access_log /var/log/nginx/cyberia.moe.access.log;

        location / {
                include uwsgi_params;
                uwsgi_pass unix:///home/vicfred/cyberia.moe/cyberia.moe.sock;
        }
}
```

Para activar el sitio hacemos un symlink al directorio correspondiente

```
$ cd /etc/nginx/sites/available
$ ln -s cyberia.moe.conf ../sites-enabled/cyberia.moe.conf
```

# Creacion de una aplicacion de flask

Vamos a crear una aplicacion de flask en ~, primero instalamos un entorno virtual e instalamos flask ahi

```
$ cd ~
$ mkdir cyberia.moe
$ cd cyberia.moe
$ python3.7 -m venv venv
$ source venv/bin/activate
$ pip install --upgrade pip
$ pip install flask
$ pip freeze > requirements.txt
```

Creamos una aplicacion de flask. Hacemos un archivo `app.py` con:

```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'
```

La probamos con:

```
$ python app.py
```

En el mismo directorio creamos `wsgi.py` con:

```
from app import app as application

if __name__ == "__main__":
    application.run()
```

Cambiamos los permisos del directorio:

```
# chown uwsgi:uwsgi /home/vicfred/cyberia.moe/
```

# Configuracion de uwsgi


Creamos el archivo de configuracion

```
# cd /etc/conf.d/
# cp uwsgi uwsgi.cyberia.moe
```
Ejemplo de archivo de configuracion

```
# Distributed under the terms of the GNU General Public License v2

# YOU SHOULD ONLY MODIFY THIS FILE IF YOU USE THE UWSGI EMPEROR MODE!
# IF YOU WANT TO RUN A SINGLE APP INSTANCE, CREATE A COPY AND MODIFY THAT INSTEAD!

# Path (or name) of UNIX/TCP socket to bind to
# Example : UWSGI_SOCKET=127.0.0.1:1234
UWSGI_SOCKET=/home/vicfred/cyberia.moe/cyberia.moe.sock

# Enable threads? (1 = yes, 0 = no). The default is 0
#
UWSGI_THREADS=1

# The path to your uWSGI application.
#
UWSGI_PROGRAM=/home/vicfred/cyberia.moe/wsgi.py

# The path to your uWSGI xml config file.
#
UWSGI_XML_CONFIG=

# The number of child processes to spawn. The default is 1.
#
UWSGI_PROCESSES=1

# The log file path. If empty, log only errors
#
UWSGI_LOG_FILE=/var/log/uwsgi/cyberia.moe

# If you want to run your application inside a chroot then specify the
# directory here. Leave this blank otherwise.
#
UWSGI_CHROOT=

# If you want to run your application from a specific directiory specify
# it here. Leave this blank otherwise.
#
UWSGI_DIR=/home/vicfred/cyberia.moe/

# PIDPATH folder mode (/run/uwsgi_${PROGNAME})
UWSGI_PIDPATH_MODE=0750

# The user to run your application as. If you do not specify these,
# the application will be run as user root.
#
UWSGI_USER=uwsgi

# The group to run your application as. If you do not specify these,
# the application will be run as group root.
#
UWSGI_GROUP=uwsgi

# Run the uwsgi emperor which loads vassals dynamically from this PATH
# see http://projects.unbit.it/uwsgi/wiki/Emperor
# The advised Gentoo folder is /etc/uwsgi.d/
UWSGI_EMPEROR_PATH=/etc/uwsgi.d/

# Emperor PIDPATH folder mode (/run/uwsgi)
UWSGI_EMPEROR_PIDPATH_MODE=0770

# The group the emperor should run as. This is different from the UWSGI_GROUP
# as you could want your apps share some sockets with other processes such as
# www servers while preserving your emperor logs from being accessible by them.

# This fixes the 'permission denied problem with nginx'
# in gentoo the nginx server is run with user 'nginx' and group 'nginx'
UWSGI_EMPEROR_GROUP=nginx

# Additional options you might want to pass to uWSGI
#
UWSGI_EXTRA_OPTIONS=
```

Y creamos un archivo de configuracion para nuestro sitio en `/etc/uwsgi.d/cyberia.moe.ini`:

```
[uwsgi]

plugins = python37
# path to my project
chdir           = /home/vicfred/cyberia.moe
# the virtualenv (full path)
home            = /home/vicfred/cyberia.moe/venv
# wsgi file
wsgi-file       = wsgi.py

# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 4
# the socket (use the full path to be safe
socket          = /home/vicfred/cyberia.moe/cyberia.moe.sock
# ... with appropriate permissions - may be needed
chmod-socket    = 664
uid             = nginx
gid             = nginx
# clear environment on exit
vacuum          = true
```

Creamos un servicio

```
# cd /etc/init.d/
# ln -s uwsgi uwsgi.cyberia.moe
```

Ahora podemos iniciar nginx y uwsgi

```
# /etc/init.d/nginx start
# /etc/init.d/uwsgi.cyberia.moe start
```

Y podemos hacer que inicien cuando inicie el sistema

```
rc-update add nginx default
rc-update add uwsgi.cyberia.moe default
```

# Referencias
https://uwsgi-docs.readthedocs.io/en/latest/

https://uwsgi-docs.readthedocs.io/en/latest/Emperor.html

https://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html

https://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html

https://stackoverflow.com/questions/22071681/permission-denied-nginx-and-uwsgi-socket