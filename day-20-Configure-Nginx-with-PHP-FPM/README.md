# Day 20 - Configure Nginx with PHP-FPM 8.2 Using Unix Socket

## Task

Set up a PHP-based application on App Server 2 using Nginx and PHP-FPM.

### Requirements

* Install Nginx on App Server 2
* Configure Nginx to listen on port `8098`
* Use `/var/www/html` as the document root
* Install PHP-FPM version `8.2`
* Configure PHP-FPM to use Unix socket `/var/run/php-fpm/default.sock`
* Integrate Nginx with PHP-FPM
* Verify using `curl http://stapp02:8098/index.php`

---

## Server

* App Server 2: `stapp02`

---

## Steps Performed

### 1. Installed Nginx

```bash
sudo yum install -y nginx
```

---

### 2. Installed PHP 8.2

```bash
sudo yum module reset php -y
sudo yum module enable php:8.2 -y
sudo yum install -y php php-fpm php-cli php-common
```

---

### 3. Verified PHP version

```bash
php-fpm -v
```

---

### 4. Configured PHP-FPM socket

Edited:

```bash
sudo vi /etc/php-fpm.d/www.conf
```

Set:

```ini
listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

---

### 5. Created socket directory

```bash
sudo mkdir -p /var/run/php-fpm
```

---

### 6. Configured Nginx

Edited:

```bash
sudo vi /etc/nginx/nginx.conf
```

Configured server block:

```nginx
server {
    listen       8098;
    server_name  stapp02;
    root         /var/www/html;
    index        index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include        fastcgi_params;
        fastcgi_pass   unix:/var/run/php-fpm/default.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }
}
```

---

### 7. Restarted and enabled services

```bash
sudo systemctl restart php-fpm
sudo systemctl enable php-fpm

sudo systemctl restart nginx
sudo systemctl enable nginx
```

---

## Verification

### Tested from jump host

```bash
curl http://stapp02:8098/index.php
```

Expected output:

```text
Welcome to xFusionCorp Industries!
```

---

## Explanation

* Nginx was configured as a web server on port `8098`
* PHP-FPM 8.2 processes PHP requests
* Communication between Nginx and PHP-FPM uses a Unix socket
* Proper socket permissions ensure Nginx can access PHP-FPM
* The application files were served without modification

---

## Issues Faced and Fix

### Issue 1: Wrong PHP version

* Default PHP version was installed initially
* Lab required specifically PHP 8.2

### Fix:

```bash
sudo yum module reset php -y
sudo yum module enable php:8.2 -y
```

---

### Issue 2: 502 Bad Gateway (previous experience)

* Caused by incorrect socket permissions

### Fix:

Uncommented:

```ini
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```

---

## What I Learned

* how to enforce specific package versions using yum modules
* how Nginx and PHP-FPM integrate
* how Unix sockets work for inter-process communication
* how to debug 502 errors in web applications

---

## Real-World Relevance

This setup reflects a common production architecture for PHP applications using Nginx and PHP-FPM, where correct versioning and service integration are critical for application stability.

---

## Improvement / Automation Idea

This setup can be automated using Ansible to ensure consistent PHP versioning and Nginx configuration across environments.

