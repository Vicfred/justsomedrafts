> phpbb
```nginx
#upstream php {
#	server unix:/var/run/php-fpm/php-fpm.sock;
#}

server {
	server_name cyberia.moe www.cyberia.moe;
	error_log /var/log/nginx/cyberia.moe.error.log;
	access_log /var/log/nginx/cyberia.moe.access.log;

	root /var/www/phpbb;
	index  index.php index.html index.htm;

	location / {
		try_files $uri $uri/ @rewriteapp;

		# Pass the php scripts to FastCGI server specified in upstream declaration.
		location ~ \.php(/|$) {
			include fastcgi.conf;
			fastcgi_split_path_info ^(.+\.php)(/.*)$;
			fastcgi_param PATH_INFO $fastcgi_path_info;
			fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
			fastcgi_param DOCUMENT_ROOT $realpath_root;
			try_files $uri $uri/ /app.php$is_args$args;
			fastcgi_pass 127.0.0.1:9000;
			include fastcgi.conf;
		}

		# Deny access to internal phpbb files.
		location ~ /(config\.php|common\.php|cache|files|images/avatars/upload|includes|(?<!ext/)phpbb(?!\w+)|store|vendor) {
			deny all;
			# deny was ignored before 0.8.40 for connections over IPv6.
			# Use internal directive to prohibit access on older versions.
			internal;
		}
	}

	location @rewriteapp {
		rewrite ^(.*)$ /app.php/$1 last;
	}

	# Correctly pass scripts for installer
	location /install/ {
		try_files $uri $uri/ @rewrite_installapp =404;

		# Pass the php scripts to fastcgi server specified in upstream declaration.
		location ~ \.php(/|$) {
			include fastcgi.conf;
			fastcgi_split_path_info ^(.+\.php)(/.*)$;
			fastcgi_param PATH_INFO $fastcgi_path_info;
			fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
			fastcgi_param DOCUMENT_ROOT $realpath_root;
			try_files $uri $uri/ /install/app.php$is_args$args =404;
			fastcgi_pass 127.0.0.1:9000;
			include fastcgi.conf;
		}
	}

	location @rewrite_installapp {
		rewrite ^(.*)$ /install/app.php/$1 last;
	}

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/cyberia.moe/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/cyberia.moe/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot




}

server {
    if ($host = www.cyberia.moe) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = cyberia.moe) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    listen 80;
    server_name cyberia.moe www.cyberia.moe;
    return 404; # managed by Certbot
}
```
> schemebbs
```nginx
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=cache:10m inactive=600s max_size=100m;

# Limit post requests
map $request_method $limit {
	default "";
	POST $binary_remote_addr;
}

# Creates 10mb zone in memory for storing binary IPs
limit_req_zone $limit zone=post_limit:10m rate=11r/m;

upstream http_backend {
	keepalive 20;
	server 127.0.0.1:9321;
}

server {
	error_log /var/log/nginx/mundoenfermoytriste.net.error.log;
	access_log /var/log/nginx/mundoenfermoytriste.net.access.log;
	server_name mundoenfermoytriste.net www.mundoenfermoytriste.net;
	set $prefix "/opt/schemebbs";

	# Site root, a static page
	location = / {
		rewrite ^ /static/index.html;
	}
	location = /favicon.ico {
		rewrite ^ /static/favicon.ico;
	}
	location /static/ {
		alias $prefix/static/;
	}

	# Serve S-expressions as static files
	location /sexp {
		alias $prefix/data/sexp/;
		autoindex on;
		default_type text/x-scheme;
		fancyindex on;
		fancyindex_time_format "%F %R";
		fancyindex_footer "/static/lisp.html";
	}

	location / {
		root   $prefix/data/html;
		default_type text/html;
		index  index;
		try_files $uri $uri/index @schemebbs;
	}

	location @schemebbs {
		proxy_intercept_errors on;
		proxy_http_version 1.1;
		proxy_set_header Connection "";
		proxy_set_header Accept-Encoding "";
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Clacks-Overhead "GNU John McCarthy";
		proxy_cache cache;
		proxy_cache_key $scheme$host$request_method$request_uri;
		proxy_cache_valid 200 30s;
		proxy_pass http://http_backend;
	}

	error_page 400 /400.html;
	error_page 403 /403.html;
	error_page 404 /404.html;
	error_page 405 /405.html;
	error_page 429 /429.html;
	error_page 500 /500.html;
	error_page 502 /502.html;
	error_page 503 /503.html;
	error_page 504 /504.html;

	location ~ ^/(400|403|404|405|429|500|502|503|504)\.html {
		root $prefix/static/errors;
	}

	listen [::]:443 ssl; # managed by Certbot
	listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/cyberia.moe/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/cyberia.moe/privkey.pem; # managed by Certbot
	include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
	ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


}

server {
	if ($host = www.mundoenfermoytriste.net) {
		return 301 https://$host$request_uri;
	} # managed by Certbot

	if ($host = mundoenfermoytriste.net) {
		return 301 https://$host$request_uri;
	} # managed by Certbot

	listen 80;
	listen [::]:80;
	server_name mundoenfermoytriste.net www.mundoenfermoytriste.net;
	return 404; # managed by Certbot
}
```
> /etc/init.d/schemebbs
```bash
#!/sbin/openrc-run

command="/opt/schemebbs/schemebbs.sh"
command_args="9321"
pidfile="/var/run/schemebbs.pid"
command_background=true
```
> django / uwsgi
```nginx
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
>serve static content
```nginx
server {
    server_name trucazos.net www.trucazos.net;
    error_log /var/log/nginx/trucazos.net.error.log;
    access_log off;

    root /var/www/trucazos.net;
    autoindex off;

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/cyberia.moe/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/cyberia.moe/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


}
server {
    if ($host = www.trucazos.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = trucazos.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    listen 80;
    server_name trucazos.net www.trucazos.net;
    return 404; # managed by Certbot




}
```
> port redirection
```nginx
server {
  server_name trucazos.net www.trucazos.net;
  error_log /var/log/nginx/trucazos.net.error.log;
  access_log off;

  location / {
    access_log /var/log/nginx/trucazos.net.access.log;
    proxy_pass  http://127.0.0.1:9000/;
  }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/trucazos.net/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/trucazos.net/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


}
server {
    if ($host = www.trucazos.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = trucazos.net) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


  server_name trucazos.net www.trucazos.net;

  listen 80;
    return 404; # managed by Certbot




}
```