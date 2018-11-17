# vps

## Change pass
```
# passwd root
# adduser dev
# gpasswd -a dev sudo
```
## Set Up SSH keys

check for existing SSH key

```
$ ls -l ~/.ssh/id_*.pub

```
generate a new 4096 bit SSH
```
$ ssh-keygen -t rsa -b 4096 -C "example@example.com"
```
verify
```
$ ls ~/.ssh/id_*
```

```
ssh root@168.235.85.22 C:\Users\Web\.ssh\my_server
```

## Improving the connection flow to the VPS

Create config file
```
Host my_vps_root
	HostName 168.235.85.22
	User root
	IdentityFile C:\Users\Web\.ssh\my_server
```
save file

```
ssh my_vps_root
```

## Keep active the connection to the server

```
ServerAliveInterval 120
ServerAliveCountMax 3

Host my_vps_root
	HostName 168.235.85.22
	User root
	IdentityFile C:\Users\Web\.ssh\my_server
```

## Pointing an existing domain to the VPS server

create A record
create CNMAE record

## Commands

```
ls -la

ll

cd -

touch

nano my_file.txt

cat my_file.txt // see the content from command

less my_file.txt // long content // Q to exit

mv my_file.txt / // to root 

mv my_file.txt ~/my_other_file.txt

cp

rm

rm test_*
```

Directory

```
mkdir

cp -r dir_1 dir_2

mv

rm -r test

clear

cd

cd / 
```

ctrl + c syste

## How to restart the server when required

```
reboot
```

## Manage Users

```
adduser sun

cd /home

deluser sun --remove-home

ServerAliveInterval 120
ServerAliveCountMax 3

Host my_vps_root
	HostName 168.235.85.22
	User root
	IdentityFile C:\Users\Web\.ssh\my_server

Host my_vps_root
	HostName 168.235.85.22
	User dev
	IdentityFile C:\Users\Web\.ssh\my_server_dev


sudo adduser dev sudo

!!

```

## Firewall

```
sudo ufw status
sudo ufw status verbose
sudo ufw app list
sudo ufw allow "OpenSSH"
sudo ufw enable

```
## Assigning permissions correctrly

```
sudo chmod -R 777 test/
sudo chown -R dev test/
sudo chown -R dev:dev test/

```

## Using Fail2Ban to prevent intruders in the VPS

```
$ cd /etc/fial2ban
$ ll
$ nano jail.conf
$ nano jail.local
[DEFAULT]
bantime = 3h
maxretry = 3

[sshd]
enabled = true

sudo systemctl restart fail2ban.service
sudo fail2ban-client status
sudo fail2ban-client status sshd

```

## Nginx

```
sudo ufw allow 'Nginx Full'

```
check domain.com

## Location of Nginx and its important files

```
$ cd /etc/nginx/
$ nano nginx.conf
```

## Setting up Nginx

```
sudo rm ../sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx.service
sudo cp default

sudo ln -s /etc/nginx/sites-available/site.com /etc/nginx/sites-enabled/
sudo ll ../sites-enabled/
sudo nginx -t
sudo systemctl reload nginx.service
cd /var/www
sudo rm -r html
sudo mkdir sitename.com
```

## Configure subdomains in Nginx

```
sudo cp sitename.com.conf api.sitename.com.conf
sudo ln -s /etc/nginx/sites-available/api.sitename.com.conf /etc/nginx/sites-enabled/
sudo ll ../sites-enabled/
sudo nano *.sitename.com.conf
	listen 80;
	listen [::]:80;
	api.sitename.com
sudo nginx -t
sudo systemctl reload nginx.service

cd /var/www
sudo cp -r sitename.com/ api.sitename.com/ 
sudo nano */index.html
```

## MySQL, MariaDB

```
sudo apt update
sudo  apt install mariadb-server mariadb-client
mysql --version

sudo mysql -u root -p

create databse sitename_com;
``

Secure database
```
sudo mysql_secure_installation
```
Create users
```
mysql

```
CREATE USER 'user'@'localhost' IDENTIFIED BY 'new_password_here';
GRANT ALL ON devdb.* TO 'dev'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
select user from mysql.user
FLUSH PRIVILEGES;
```

## Installing and configuring PHP

```
sudo apt install php-fpm
php --version

cd /etc/php
ll
cd /etc/php/7.2
ll
cd /fpm
sudo nano php.ini

cd /var/log
sudo nano php7.2-fpm.log
```

## Securing PHP

```
sudo nano /etc/php/7.2/fpm/php.ini
	cgi.fix_pathinfo=0 // for security

sudo systemctl reload php7.2-fpm.service
```

## Sending requests from Nginx to PHP-FPM

```
nano /etc/nginx/sites-available/*sitename.com.conf
	location ~ \.php$ {
			include snippets/fastcgi-php.conf;
			fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;

sudo nginx -t
sudo systemctl reload nginx.service

cd /var/www/
sudo nano sitename.com/index.php
```

## Nginx safe and efficient

```
sudo nano /etc/nginx/sites-available/sitename.com.conf

	# deny access to .htaccess files, if Apache's document root
	# concurs with nginx's one
	#
	location ~ /\.ht {
	       deny all;
	}
	location ~ /\.git {
	       deny all;
	}
sudo nginx -t
sudo systemctl reload nginx.service
```

## Hiding the Nginx signature in the responses

```
sudo nano /etc/nginx/nginx.conf
	server_tokens off;
sudo nginx -t
sudo systemctl reload nginx.service
```

## Avoiding clickjacking on Nginx sites

```
sudo nano /etc/nginx/nginx.conf
	##
	# Security Settings
	##

	# Avoid iFrames for clickjacking attacks
	add_header X-Frame-Options SAMEORIGIN;

sudo nginx -t
sudo systemctl reload nginx.service

```

## Avoiding MIME confusion attakcs

```
sudo nano /etc/nginx/nginx.conf
	# Avoid MIME type sniffing
  add_header X-Content-Type-Options nosniff;
sudo nginx -t
sudo systemctl reload nginx.service

```

## Avoiding XSS attacks on Nginx sites

```
sudo nano /etc/nginx/nginx.conf
	# Avoid XSS attacks
  add_header X-XSS-Protection "1;mode=block";
sudo nginx -t
sudo systemctl reload nginx.service
```

## Enabling compress in Nginx with Gzip

```
sudo nano /etc/nginx/nginx.conf
	##
	# Gzip Settings
	##

	gzip on;

	gzip_vary on;
	# gzip_proxied any;
	gzip_comp_level 6;
	gzip_buffers 16 8k;
	gzip_http_version 1.1;
	gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
sudo nginx -t
sudo systemctl reload nginx.service
```

## Mitigating DoS and DDoS attacks on Nginx

```
sudo nano /etc/nginx/nginx.conf
	##
	# DoS and DDoS Protection Settings
	##

	#Define limit connection zone called conn_limit_per_ip with memory size 15m based on the unique IP
	limit_conn_zone $binary_remote_addr zone=conn_limit_per_ip:15m;

	#Define limit request to 40/sec in zone called req_limit_per_ip memory size 15m based on IP
	limit_req_zone $binary_remote_addr zone=req_limit_per_ip:15m rate=40r/s;

	#Using the zone called conn_limit_per_ip with max 40 connections per IP
	limit_conn conn_limit_per_ip 40;

	#Using the zone req_limit_per_ip with an exceed queue of size 40 without delay for the 40 additonal
	limit_req zone=req_limit_per_ip burst=40 nodelay;

	#Do not wait for the client body or headers more than 5s (avoid slowloris attack)
	client_body_timeout 5s;
	client_header_timeout 5s;
	send_timeout 5;

	#Establishing body and headers max size to avoid overloading the server I/O
	client_body_buffer_size 256k;
	client_header_buffer_size 2k;
	client_max_body_size 3m;
	large_client_header_buffers 2 2k;
sudo nginx -t
sudo systemctl reload nginx.service

```
