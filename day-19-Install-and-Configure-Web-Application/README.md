# Day 19 - Configure Apache to Host Two Static Websites

## Task

Prepare App Server 1 to host two static websites using Apache.

### Requirements

* Install the `httpd` package and dependencies
* Configure Apache to serve on port `6100`
* Deploy two static sites:

  * `official` → available at `http://localhost:6100/official/`
  * `demo` → available at `http://localhost:6100/demo/`
* Verify both sites using `curl`

---

## Server

* App Server 1: `stapp01`

---

## Steps Performed

### 1. Installed Apache

```bash
sudo yum install -y httpd
```

### 2. Updated Apache to use port 6100

Edited the Apache configuration file:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Changed:

```apache
Listen 80
```

to:

```apache
Listen 6100
```

### 3. Copied website backups from jump host

Copied the two website directories from the jump host into Apache’s document root:

```bash
scp -r thor@jump-host:/home/thor/official /var/www/html/
scp -r thor@jump-host:/home/thor/demo /var/www/html/
```

### 4. Set correct ownership

```bash
sudo chown -R apache:apache /var/www/html/official
sudo chown -R apache:apache /var/www/html/demo
```

### 5. Started and enabled Apache

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

---

## Verification

### Tested the official site

```bash
curl http://localhost:6100/official/
```

Output included:

```html
<h1>KodeKloud</h1>
<p>This is a sample page for our official website</p>
```

### Tested the demo site

```bash
curl http://localhost:6100/demo/
```

Output included:

```html
<h1>KodeKloud</h1>
<p>This is a sample page for our demo website</p>
```

---

## Explanation

Apache was configured to listen on port `6100` instead of the default port `80`. Two static website directories were copied into `/var/www/html/`, allowing Apache to serve them under path-based URLs:

* `/official/`
* `/demo/`

Because both folders existed under the Apache document root, Apache automatically served each site through the matching URL path.

---

## What I Learned

* how to configure Apache to use a custom port
* how to host multiple static websites under one web server
* how Apache maps URL paths to directories in the document root
* how to validate web server configuration using `curl`

---

## Real-World Relevance

This setup reflects how teams can host multiple static applications or environments (such as production and demo) on the same server using path-based access.

---

## Improvement / Automation Idea

This deployment could be automated with Ansible by copying website content, configuring Apache, and managing service startup consistently across environments.

