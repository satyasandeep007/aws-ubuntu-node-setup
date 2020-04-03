# satyasandeep007 
## AWS ssh Nodejs Commands on Ubuntu

Create Amazon Ubuntu 18.04 Instance
Login using ssh on pc

### Install Nginx

sudo apt update
sudo apt install nginx
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
sudo ufw enable 
sudo ufw allow OpenSSH
systemctl status nginx
curl -4 icanhazip.com

### Node Setup

cd ~
curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
nano nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt install nodejs
nodejs -v
npm -v
sudo apt install build-essential

### Check Nginx and Node using hello.js

cd ~
nano hello.js

now copy paste following

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
node hello.js
sudo npm install pm2@latest -g
pm2 start hello.js

### Deploy Node App from git

git clone https://github.com/satyasandeep007/Weather-App.git
cd Weather-App
ls -l
npm install
sudo npm update
pm2 start server.js

pm2 list

### Assigning port to Node App

sudo nano /etc/nginx/sites-available/default

inside server block
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
    
sudo nginx -t
sudo systemctl restart nginx

# To restart server after commit in repository

Login
cd Weather-App
git pull origin master
UserName:
Password:
pm2 restart server.js
pm2 log




