## NGINX and PHP-FPM Installation Steps

## Update the System
Ensure your system is up to date:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

## Install PHP-FPM

```bash
sudo apt install php8.1-fpm -y
sudo systemctl start php8.1-fpm
sudo systemctl enable php8.1-fpm
```


## Create Nginx Configuration File

```bash
# Redirect HTTP traffic to HTTPS
server {
    listen 80;
    server_name alpha.in;
    return 301 https://$host$request_uri;
}

# HTTPS server block for Keycloak reverse proxy
server {
    listen 443 ssl http2;
    server_name alpha.in;

    root /var/www/;
    index index.php index.html index.htm;    

    # SSL settings
    ssl_certificate /etc/nginx/ssl/erp/certificate.crt;
    ssl_certificate_key /etc/nginx/ssl/erp/private.key;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    ssl_session_tickets off;

    # Set timeout directives
    proxy_connect_timeout 3600s;
    proxy_send_timeout 3600s;
    proxy_read_timeout 3600s;
    send_timeout 3600s;
    client_max_body_size 1024M;

    # Custom Error Pages
    error_page 404 /404.html;  # Custom 404 page
    error_page 403 /403.html;  # Custom 403 page

    location = /404.html {
        root /var/www/;  # Path to the 404.html file
        internal;
    }

    location = /403.html {
        root /var/www/;  # Path to the 403.html file
        internal;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        
        fastcgi_connect_timeout 3600s;
        fastcgi_send_timeout 3600s;
        fastcgi_read_timeout 3600s;
        
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~* \.(js|css)$ {
        access_log off;
        log_not_found off;
    }

    location ~* \.(jpg|jpeg|png|gif|bmp|webp|svg)$ {
        access_log off;
        log_not_found off;
    }
}
```
## Enable the Configuration

```bash
sudo ln -s /etc/nginx/sites-available/alpha.in.conf /etc/nginx/sites-enabled/
```

## Set Up SSL Certificates

```bash 
sudo mkdir -p /etc/nginx/ssl/erp
sudo cp /path/to/your/certificate.crt /etc/nginx/ssl/erp/certificate.crt
sudo cp /path/to/your/private.key /etc/nginx/ssl/erp/private.key
sudo chmod 600 /etc/nginx/ssl/erp/certificate.crt
sudo chmod 600 /etc/nginx/ssl/erp/private.key
```

## Test the Nginx Configuration

```bash
sudo nginx -t
```

##  Reload Nginx
```bash
sudo systemctl reload nginx
```

## Set Up the Web Root Directory
```bash
sudo mkdir -p /var/www
sudo chown -R $USER:$USER /var/www
```
**Add your index.html, 404.html, and 403.html files to /var/www/
