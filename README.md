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

### Step 3. Configuration

- The location where the configuration file is `/etc/nginx/`
- The main configuration file: `/etc/nginx/nginx.conf`

### Step 4. Create a new directroty `/web/html/nginx-2420`

- That will act as your project root
- The directory where your website documents will be stored
- `sudo mkdir -p /web/html/nginx-2420`

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

server {
listen 80;
listen [::]:80;
server_name domainname2.tld;
root /usr/share/nginx/domainname2.tld/html;
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
