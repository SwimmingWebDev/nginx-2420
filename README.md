# Arch Linux server running on Digital Ocean(DO)

### Step 1. Install **Vim** and **Nginx** in Arch on DO

❗Before installing **Vim** and **Nginx**, update `pacman`<br>

- `sudo pacman -Syu`

- Install **Vim** and **Nginx**<br>
  `sudo pacman -S vim nginx`

👉 **Nginx** - a free, open-source, high-performance HTTP web server and reverse proxy

### Step 2. Running **Nginx**

- enable **Nginx**<br>
  `sudo systemctl enable nginx`

- start **Nginx**<br>
  `sudo systemctl start nginx`

- Check if **Nginx** is running<br>
  `systemctl status nginx.service`

- Restart `nginx.service` to apply any changes<br>
  `systemctl restart nginx`

### Step 3. Create a new directroty `/web/html/nginx-2420`

- That will act as your project root
- The directory where your website documents will be stored
- `sudo mkdir -p /web/html/nginx-2420`

### Step 4. Edit Configuration

- The location where the configuration file is `/etc/nginx/`
- The main configuration file: `/etc/nginx/nginx.conf`

```bash
user http;
worker_processes auto;
worker_cpu_affinity auto;

events {
    multi_accept on;
    worker_connections 1024;
}

http {
    charset utf-8;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    server_tokens off;
    log_not_found off;
    types_hash_max_size 4096;
    client_max_body_size 16M;

    # MIME
    include mime.types;
    default_type application/octet-stream;

    # logging
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log warn;

    # load configs
    include /etc/nginx/sites-available/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

### Step 5. Setup a separate server block `nginx-2420`

❗ It should **NOT** be part of the `nginx.conf` file

- create the following diretories

```
# mkdir /etc/nginx/sites-available
# mkdir /etc/nginx/sites-enabled
```

- create `nginx-2420.conf` and add the contents below

```bash
server {
listen 80;
listen [::]:80;
server_name domainname1.tld;
root /web/html/nginx-2420;
location / {
index index.html;
}
}
```

- To enable a site, simply create a symlink

```
sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/nginx-2420.conf
```

- Reload/restart `nginx.service` to enable changes to the site's configuration
  - Reload: `sudo systemctl reload nginx`
  - Restart: `sudo systemctl restart nginx`

### Step 6. Create **index.html** to display main page

👉 The location where the `index.html`: `/web/html/nginx-2420/html`

- create `index.html`: `sudo touch index.html`
- edit `index.html` : `sudo vim index.html`
- add contents below into `index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>2420</title>
    <style>
      * {
        color: #db4b4b;
        background: #16161e;
      }
      body {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
        margin: 0;
      }
      h1 {
        text-align: center;
        font-family: sans-serif;
      }
    </style>
  </head>
  <body>
    <h1>All your base are belong to us</h1>
  </body>
</html>
```

### Example - screenshot

[![screenshot]](./screenshot/screenshot.png)
