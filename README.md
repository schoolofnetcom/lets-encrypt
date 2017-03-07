# Lets Encrypt

## Instalação

	apt-get update
	apt-get install letsencrypt

## Ativar módulo ssh no apache

	sudo a2enmod ssl

## Gerar certificado

	letsencrypt certonly
	letsencrypt certonly --apache

## Renovar certificado

	letsencrypt renew

## Expiração

90 dias

envia aviso:

 - faltando 20 para expiração
 - faltando 10 para expiração
 - faltando 1 para expiração

## Limites

20 dominios por semana
100 dominios por arquivo (subdominios)

total geral: 2000 subdominios por semana


## Links para saber mais

 - https://letsencrypt.org/docs/rate-limits/
 - https://letsencrypt.org/docs/faq/
 - https://letsencrypt.org/docs/

## Redirecionamento apache

	#.htaccess
	RewriteCond %{HTTPS} off
	RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]


## Exemplo nginx

	server {

	    listen 443 ssl default_server;
	    listen [::]:443 ssl default_server;

	    root /var/www/html;

	    index index.html index.htm index.nginx-debian.html;

	    server_name nginxssl.cakephpbrasil.com.br;

	    ssl_certificate /etc/letsencrypt/live/dominio/fullchain.pem;
	    ssl_certificate_key /etc/letsencrypt/live/dominio/privkey.pem;
	    ssl_trusted_certificate /etc/letsencrypt/live/dominio/fullchain.pem;
	    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;

	    location / {
	            # First attempt to serve request as file, then
	            # as directory, then fall back to displaying a 404.
	            try_files $uri $uri/ =404;
	    }
	}

### Redirecionamento

	server {
	    listen 80;
	    server_name seusite.cakephpbrasil.com.br;
	    location / {
	            return 301 https://$server_name$request_uri;
	    }
	}
