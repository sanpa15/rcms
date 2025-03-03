# Nginx Virtual Host Configuration for `alpha.in`

This repository contains an Nginx virtual host configuration file for the domain `alpha.in`. The configuration handles HTTP to HTTPS redirection, SSL/TLS settings, custom error pages, and PHP processing.

## Configuration Overview

### 1. **HTTP to HTTPS Redirection**
   - All HTTP traffic (port 80) is redirected to HTTPS (port 443) using a `301` permanent redirect.
   - This ensures secure communication for all users.

### 2. **HTTPS Server Block**
   - Listens on port 443 with SSL/TLS enabled.
   - Uses HTTP/2 for improved performance.
   - SSL certificates are provided via:
     - `ssl_certificate`: Path to the SSL certificate (`/etc/nginx/ssl/erp/certificate.crt`).
     - `ssl_certificate_key`: Path to the private key (`/etc/nginx/ssl/erp/private.key`).
   - Supports TLSv1.2 and TLSv1.3 protocols for secure communication.

### 3. **Timeouts and Limits**
   - Timeout settings are configured for proxy and client connections:
     - `proxy_connect_timeout`, `proxy_send_timeout`, `proxy_read_timeout`, and `send_timeout` are set to `3600s` (1 hour).
   - `client_max_body_size` is set to `1024M` (1GB) to allow large file uploads.

### 4. **Custom Error Pages**
   - Custom error pages for `404` (Not Found) and `403` (Forbidden) are served from `/var/www/`.
   - The error pages are located at `/var/www/404.html` and `/var/www/403.html`.

### 5. **PHP Processing**
   - PHP files are processed using PHP-FPM (FastCGI Process Manager) with the following settings:
     - PHP-FPM socket: `/var/run/php/php8.1-fpm.sock`.
     - Timeout settings for FastCGI are also set to `3600s`.
   - The `SCRIPT_FILENAME` parameter is dynamically set to handle PHP scripts.

### 6. **Static File Handling**
   - JavaScript (`js`), CSS (`css`), and image files (`jpg`, `jpeg`, `png`, `gif`, `bmp`, `webp`, `svg`) are served with:
     - Access logs disabled (`access_log off`).
     - Logging for missing files disabled (`log_not_found off`).

## File Structure
- **Nginx Configuration File**: `alpha.in.conf`
  - Located in `/etc/nginx/sites-available/` (or `/etc/nginx/conf.d/` depending on your setup).
  - Symlinked to `/etc/nginx/sites-enabled/` for activation.

- **SSL Certificates**:
  - Certificate: `/etc/nginx/ssl/erp/certificate.crt`
  - Private Key: `/etc/nginx/ssl/erp/private.key`

- **Custom Error Pages**:
  - `404.html`: `/var/www/404.html`
  - `403.html`: `/var/www/403.html`

## How to Use
1. Place the `alpha.in.conf` file in your Nginx configuration directory (e.g., `/etc/nginx/sites-available/`).
2. Create a symlink to enable the configuration:
   ```bash
   sudo ln -s /etc/nginx/sites-available/alpha.in.conf /etc/nginx/sites-enabled/


   
---

### Explanation of the Configuration

1. **HTTP to HTTPS Redirection**:
   - The first `server` block listens on port 80 (HTTP) and redirects all traffic to HTTPS using a `301` redirect. This ensures that users always access the site securely.

2. **HTTPS Server Block**:
   - The second `server` block listens on port 443 (HTTPS) and configures SSL/TLS settings.
   - It uses HTTP/2 for better performance and security.
   - SSL certificates and keys are specified using `ssl_certificate` and `ssl_certificate_key`.

3. **Timeouts and Limits**:
   - Timeout settings are extended to `3600s` (1 hour) to handle long-running processes or large file uploads.
   - `client_max_body_size` is set to `1024M` to allow large file uploads.

4. **Custom Error Pages**:
   - Custom error pages for `404` and `403` are served from `/var/www/`. These pages are internal, meaning they are not accessible directly by users.

5. **PHP Processing**:
   - PHP files are processed using PHP-FPM via a Unix socket (`/var/run/php/php8.1-fpm.sock`).
   - FastCGI timeouts are also set to `3600s` to match the proxy timeouts.

6. **Static File Handling**:
   - Static files like JavaScript, CSS, and images are served with logging disabled to reduce log clutter.

---

```bash
# Redirect HTTP traffic to HTTPS
server {
    listen 80;
    server_name alpha.radianterp.in;
    return 301 https://$host$request_uri;
}

# HTTPS server block for Keycloak reverse proxy
server {
    listen 443 ssl http2;
    server_name alpha.radianterp.in;

    root /var/www/dbs/rcms;
    index index.php index.html index.htm;    

    # SSL settings
    ssl_certificate /etc/nginx/ssl/radianterp/certificate.crt;
    ssl_certificate_key /etc/nginx/ssl/radianterp/private.key;
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
        root /var/www/dbs/rcms;  # Path to the 404.html file
        internal;
    }

    location = /403.html {
        root /var/www/dbs/rcms;  # Path to the 403.html file
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
