# NGINX Settings
This is just a temporary document just to record the procedure of setting up https on a DigitalOcean droplet.
Everything will be moved to either GCP or AWS. 

Droplet Environment:
* Ubuntu 16.04.4 x64

Table of Contents:
* Install NGINX
* Configure NGINX to serve Clad-Up Viewer(Aframe)

## Install NGINX
Guide used for [installment](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04).
```sh
# Upgrade and update!
$ sudo apt-get update && sudo apt-get upgrade

# Install nginx
$ sudo apt-get install nginx
```

### NGINX Configuration
* `/etc/nginx`: Configuration directory
* `/etc/nginx/nginx.conf`: Global configuration
* `/etc/nginx/sites-available`, `etc/nginx/sites-enabled`: Both are connected, for per-server block configurations
* `/etc/nginx/snippets`: For refactoring configurations
* `/var/log/nginx/access.log`, `/var/log/nginx/error.log`: Access and error log defaults

## Configure NGINX to service Clad-Up Viewer(Aframe)
* http configuration
* https configuration

### http configuration
Inside: `/etc/nginx/sites-available/clad-up`
```nginx
server {
    listen 80;
    server_name www.clad-up.com clad-up.com;

    location / {
        proxy_pass http://localhost:8080/;
    }
}
```

```sh
# Check NGINX syntax
$ sudo nginx -t

# Link with sites-available and sites-enabled
$ sudo ln -s /etc/nginx/sites-available/clad-up /etc/nginx/sites-enabled

# Restart NGINX
$ sudo systemctl restart nginx
```

### https configuration
* Install and configure Certbot
```sh
# Add repository
$ sudo add-apt-repository ppa:certbot/certbot

# Update packages to bring repository applications
$ sudo apt-get update

# Install Certbot
$ sudo apt-get install python-certbot-nginx

# Obtain SSL Certificate
$ sudo certbot --nginx -d clad-up.com -d www.clad-up.com
```

Check https
* https://www.ssllabs.com/ssltest/analyze.html?d=clad-up.com
* https://www.ssllabs.com/ssltest/analyze.html?d=www.clad-up.com

