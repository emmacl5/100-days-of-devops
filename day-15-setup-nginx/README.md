# Day 15 - Setup SSL for Nginx

## Task

Prepare App Server 2 for application deployment by installing and configuring Nginx with SSL.

### Requirements

* Install and configure Nginx on App Server 2
* Use the provided self-signed certificate and key from `/tmp/nautilus.crt` and `/tmp/nautilus.key`
* Create an `index.html` file with content `Welcome!`
* Verify HTTPS access using `curl -Ik https://<app-server-name>/`

---

## Server

* App Server 2: `stapp02`

---

## Steps Performed

### 1. Installed Nginx

```bash
sudo yum install -y nginx
```

### 2. Started and enabled Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 3. Moved SSL certificate and key to an appropriate location

```bash
sudo mkdir -p /etc/nginx/ssl
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```

### 4. Configured Nginx for HTTPS

Updated the Nginx configuration to use the provided SSL certificate and key:

```nginx
server {
    listen       443 ssl;
    server_name  stapp02;

    ssl_certificate     /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    location / {
        root   /usr/share/nginx/html;
        index  index.html;
    }
}
```

### 5. Created the application landing page

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

### 6. Restarted Nginx

```bash
sudo systemctl restart nginx
```

---

## Verification

### Local verification on App Server 2

```bash
curl -Ik https://localhost
```

Result:

```text
HTTP/1.1 200 OK
Server: nginx/1.20.1
```

### Final verification from jump host

```bash
curl -Ik https://stapp02
```

Expected:

```text
HTTP/1.1 200 OK
```

---

## Explanation

* Nginx was installed to act as the web server
* The provided self-signed SSL certificate and key were placed under `/etc/nginx/ssl`
* Nginx was configured to listen on port `443` with SSL enabled
* A simple `index.html` page was created under the Nginx document root
* `curl -Ik` was used to test HTTPS connectivity while ignoring self-signed certificate validation

---

## What I Learned

* how to install and configure Nginx
* how to deploy SSL certificates in Nginx
* how to serve secure HTTPS content
* how to validate an HTTPS endpoint with `curl`

---

## Real-World Relevance

This mirrors a common production setup where web servers are configured with SSL certificates to securely serve applications over HTTPS.

---

## Improvement / Automation Idea

This entire setup can be automated using Ansible for consistent Nginx and SSL deployment across environments.

