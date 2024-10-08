# Customisation steps for Nginx and PHP -fpm logs

### Step 1:customize the nginx logs

 In your nginx.conf file, locate the http block and add the custom log_format directive under the Virtual Host Configs section_
```cmd
##vim /etc/nginx/nginx.conf

log_format custom '$time_local - $request_id - $remote_addr $remote_user code=$status body_bytes=$body_bytes_sent '
                                'milliseconds=$msec request_time=$request_time upstream_time=$upstream_response_time connection=$connection connection_request=$connection_requests $request '
                               '$http_referer $http_x_forwarded_for  $http_user_agent';
   ```


### step 2:Add the following lines to the your vhost.conf file
 ```cmd

   # Enable custom logging
    access_log /var/log/nginx/yourdomain.access.log custom;
    error_log /var/log/nginx/yourdomain.error.log;
  ```
### step 3:Test and Apply the Configuration
 ```cmd
   nginx -t
 ```
### step 4: Restart the nginx service & check the status of the service
```cmd
sudo systemctl  restart nginx
sudo systemctl status nginx
```

## Customize PHP-FPM Error Logs

### 1 :Edit the PHP-FPM Pool Configuration 
```cmd
#vim /etc/php/8.1/fpm/pool.d/www.conf

php_flag[display_errors] = off
php_admin_value[error_log] = /var/log/php-fpm/error.log
php_admin_flag[log_errors] = on
php_admin_value[error_reporting] = E_ALL & ~E_DEPRECATED & ~E_STRICT
slowlog = /var/log/php-fpm/slowlog-www.log
```
### 2 :Edit the fpm php.ini file 
```cmd
#vim /etc/php/8.1/fpm/php.ini

error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
log_errors = On
ignore_repeated_errors = On
ignore_repeated_source = Off
html_errors = Off
```
### 3:After making these changes, restart PHP-FPM Service

```cmd
sudo systemctl restart php8.1-fpm
```
