## AWS Node Server Setup on Ubuntu

Create Amazon Ubuntu 18.04 Instance
Login using ssh on pc

### Install Nginx

```
sudo apt update
sudo apt install nginx

```

```
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status

```

```
sudo ufw enable
sudo ufw allow OpenSSH
```

```
systemctl status nginx
curl -4 icanhazip.com

```

### Node Setup

```
cd ~
curl -sL https://deb.nodesource.com/setup_12.x -o nodesource_setup.sh
nano nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt install nodejs
nodejs -v
npm -v
sudo apt install build-essential

```

# For TypeScript

```
sudo pm2 install typescript
sudo pm2 install @types/node
npm install typescript -g
```

### Check Nginx and Node using hello.js

```
cd ~
nano hello.js
```

Now copy paste following

```
const http = require('http');

const hostname = 'localhost';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

```

Start hello

```
node hello.js
```

```
sudo npm install pm2@latest -g
pm2 start hello.js

```

### Deploy Node App from git

```
git clone https://github.com/satyasandeep007/Weather-App.git
```

Enter Username and password to verify that you have access to the repository

```
cd Weather-App
ls -l
npm install
sudo npm update
pm2 start server.js
```

```
For TypeScript
pm2 start src/server.ts
```

```
pm2 list
```

### Assigning port to Node App

```
sudo nano /etc/nginx/sites-available/default
```

Copy paste inside server block

```
 location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```

```
sudo nginx -t
sudo systemctl restart nginx
```

# To restart server after commit in repository

Login to server via SSH

```
cd Weather-App
git pull origin master
```

Enter Username and password to verify that you have access to the repository
Add Commit message if you are not master

```
pm2 restart server.js
pm2 log
```

# How to add SSL on Ubuntu 18.04 to Domain Name

## Install required dependencies

```
sudo add-apt-repository ppa:certbot/certbot
sudo apt install python-certbot-nginx
```

## Add ssl port as 443

```
sudo nano /etc/nginx/sites-available/default

server {
        listen 443 ssl default_server;
        listen [::]:443 ssl default_server;

        server_name sub.domain.in;
        # Add Your domain name

        location / {
                proxy_pass http://localhost:5005;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
     root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

}
```

## Save the file and restart Nginx

```
sudo nginx -t
sudo systemctl reload nginx
```

## check Status and delete HTTP

```
sudo ufw status
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
sudo ufw status
```

## Generate key files

```
sudo certbot --nginx -d sub.domain.in -d www.sub.domain.in
```

## Initiate Dry run

```
sudo certbot --nginx -d sub.domain.in
```

## Success

Check inside sites-availble server-block 2 files will be availble there & also check your domain using https tag.
