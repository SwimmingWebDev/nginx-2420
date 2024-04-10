# Tutorial for firewall using UFW and reverse proxy server to backend

### Step 1. Install **UFW**

❗Before installing a package, make sure to update `pacman`<br>

- update `pacman` : `sudo pacman -Syu`
- install `ufw` : `sudo pacman -S ufw`

👉 What is **UFW**? - Uncomplicated Firewall, a prograom for managing a netfilter firewall

### Step 2. Configure **UFW**

- allow ssh : `sudo ufw allow ssh`
  - You can also port numbers to allow ssh : `sudo ufw allow 22`
- allow http : `sudo ufw allow http`
- enable rules above : `sudo ufw enable`
- check for status : `sudo ufw status`
- You can get extra information, including the default policies<br>
  `ufw status verbose`

### Step 3. Place **backend** binary

- `sudo mkdir -p bin/backend`

### Step 4. transfer backend `hello-server` to your servers with `sftp`

- connect to your remote servers using the `sftp` utility<br>
  `sftp -i ~/.ssh/do-key user@ip-address`
- Upload file : `put <file-path>`

❗Make sure to place `hello-server` into `$HOME/bin/backend`

### Step 5. Create a a new service file to run the backend

- `sudo touch /etc/systemd/system/backend.service`

### Step 6. Edit `backend.service`

```bash
[Unit]
Description=Backend Service

[Service]
Type=notify
ExecStart=/bin/backend/hello-server

[Install]
WantedBy=multi-user.target

```

### Step 7. Edit nginx server block

- nginx server : `/etc/nginx/sites-available/nginx-2420`

```bash
server {
listen 80;
server_name _;
root /web/html/nginx-2420;
location / {
    index index.html index.htm;
    }
}

location /backend {
    proxy_pass <http://127.0.0.1:8080>;
}

```

### Step 8. Test **backend**

- Replace the ip address below with your own.<br>
  `curl http://137.184.181.176`
  ❗These commands should be run from your host machine, not your server

```bash
curl http://137.184.181.176/backend/hey`
```

```bash
curl -X POST -H "Content-Type: application/json" \
-d '{"message": "Hello from your server"}' \
http://137.184.181.176/backend/echo
```
