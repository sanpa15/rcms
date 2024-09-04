---

# Hardening the php and php-fpm service
 
1. **Edit and add the PHP Configuration Files** 
  
   ```bash
   #vim /etc/php/8.1/fpm/php.ini
   #vim /etc/php/8.1/cli/php.ini
   
   disable_functions = exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source
   allow_url_fopen = Off
   allow_url_include = Off
   expose_php = Off
   display_errors = Off
   log_errors = On
   error_log = /var/log/php_errors.log                   
   max_execution_time = 30
   memory_limit = 128M
   post_max_size = 8M
   upload_max_filesize = 2M
   max_input_time = 30
   session.cookie_httponly = 1
   session.cookie_secure = 1
   session.use_strict_mode = 1

   ```
2. **Restrict PHP-FPM to Specific Users**:

   ```bash
   #vim /etc/php/8.1/fpm/pool.d/www.conf

   user = www-data
   group = www-data
   slowlog = /var/log/php-fpm/slowlog-www.log
   ```
3. **Set Proper File Permissions**:
   
   ```bash
   sudo chown -R www-data:www-data /var/www/html
   sudo find /var/www/your_path -type d -exec chmod 755 {} \;
   sudo find /var/www/your_path -type f -exec chmod 644 {} \;
   ```

4. **Update Your System and PHP**:

   ```bash
   sudo apt update
   sudo apt upgrade
   sudo add-apt-repository ppa:ondrej/php #If Necessary
   sudo apt upgrade php php-fpm
   ```
   
5. **Disable Unnecessary PHP Modules**:

   ```bash
   dpkg --get-selections | grep php
   php -m
   sudo phpdismod <module_name>
   ```
   

## Proving the Hardening Effectiveness 

1. **Verify Configuration**:

   - Use a phpinfo() script to check that the changes in php.ini have been applied.
   
   ```bash
   https://demolog.radianterp.in/info.php
   ```
2. **Check Logs**:
   - Regularly review the error logs and slowlogs to detect any unusual activity or errors.
   
   ```bash
   cat /var/log/php_errors.log
   cat /var/log/php-fpm/slowlog-www.log

   ```
3. **Run Security Scans**:

   - Use security tools like Lynis or OpenVAS to scan your server for vulnerabilities.
   - These tools will identify potential weaknesses in your PHP configuration and provide recommendations for further hardening.
