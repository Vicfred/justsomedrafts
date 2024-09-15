>puffy.moe.conf
```nginx
# /etc/nginx/sites-available/puffy.moe.conf
server {
	listen 80;
	server_name puffy.moe www.puffy.moe;

	error_log /var/log/nginx/puffy.moe.error.log;
	access_log off;

	location / {
		include uwsgi_params;
		uwsgi_pass unix:///home/vicfred/django32/puffy.moe.sock;
	}
}
```

>puffy.moe.ini
```ini
# /etc/uwsgi.d/puffy.moe.ini
[uwsgi]

plugins         = python37
# path to my project
chdir           = /home/vicfred/django32/dango
# the virtualenv (full path)
home            = /home/vicfred/django32/.venv
# wsgi file
wsgi-file       = /home/vicfred/django32/dango/dango/wsgi.py

# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 4
# the socket (use the full path to be safe
socket          = /home/vicfred/django32/puffy.moe.sock
# ... with appropriate permissions - may be needed
chmod-socket    = 664
uid             = nginx
gid             = nginx
# clear environment on exit
vacuum          = true
```

>uwsgi.puffy.moe
```bash
# /etc/conf.d/uwsgi.puffy.moe
# Distributed under the terms of the GNU General Public License v2

# YOU SHOULD ONLY MODIFY THIS FILE IF YOU USE THE UWSGI EMPEROR MODE!
# IF YOU WANT TO RUN A SINGLE APP INSTANCE, CREATE A COPY AND MODIFY THAT INSTEAD!

# Path (or name) of UNIX/TCP socket to bind to
# Example : UWSGI_SOCKET=127.0.0.1:1234
#UWSGI_SOCKET=/home/vicfred/cyberia.moe/cyberia.moe.sock
UWSGI_SOCKET=/home/vicfred/django32/puffy.moe.sock

# Enable threads? (1 = yes, 0 = no). The default is 0
#
UWSGI_THREADS=1

# The path to your uWSGI application.
#
# TODO
#UWSGI_PROGRAM=/home/vicfred/cyberia.moe/wsgi.py
# This wsgi.py is provided by django.
UWSGI_PROGRAM=/home/vicfred/django32/dango/dango/wsgi.py

# The path to your uWSGI xml config file.
#
UWSGI_XML_CONFIG=

# The number of child processes to spawn. The default is 1.
#
UWSGI_PROCESSES=1

# The log file path. If empty, log only errors
#
UWSGI_LOG_FILE=/var/log/uwsgi/puffy.moe

# If you want to run your application inside a chroot then specify the
# directory here. Leave this blank otherwise.
#
UWSGI_CHROOT=

# If you want to run your application from a specific directiory specify
# it here. Leave this blank otherwise.
#
# UWSGI_DIR=/home/vicfred/cyberia.moe/
UWSGI_DIR=/home/vicfred/django32/dango

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
UWSGI_EMPEROR_GROUP=nginx

# Additional options you might want to pass to uWSGI
#
UWSGI_EXTRA_OPTIONS=
```