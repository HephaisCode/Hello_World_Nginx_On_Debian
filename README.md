# "Hello World!" with Nginx on Debian 10

[![OS badge](https://img.shields.io/badge/OS-Debian-red.svg)](https://www.debian.org)
[![Server badge](https://img.shields.io/badge/Server-Nginx-blue.svg)](https://www.nginx.com)
[![Format badge](https://img.shields.io/badge/Format-HTML-green.svg)](https://lyty.dev/html/index.html)

## Objectif 

Create Web Page for **hello-world.hephaiscode.com** with Nginx and HTML File. Access by URL  http://hello-world.hephaiscode.com .

## You need

- Server on Debian (linux distribution) with root access
- DN (Domain Name) point on Server IP (example hello-world.hephaiscode.com on 51.83.45.10)
- Terminal/console to enter instruction

## Parameters

We use :
 - hello-world.hephaiscode.com for domain name of our web page
 - 51.83.45.10 the IP address of our server computer
 - Create an user hephaistos with password 'Gkh23hglxVd47ShG43jh3h' (change the password!)
 
 
## Connect to server 

Open terminal or console and go to admin your server.

```
ssh-keygen -R 51.83.45.10
ssh -l root 51.83.45.10 
```

And enter your root password 

## Update your server

Always to be update.

```
apt-get update
apt-get -y upgrade
apt-get -y dist-upgrade

```

## Install Nginx

Install Nginx as WebServer to communicate with your server at **hello-world.hephaiscode.com**

```
apt-get -y install nginx
apt-get -y install logrotate
systemctl start nginx
```

Configure Default Nginx by rewrite index.html

```
chgrp -R www-data /var/www/html/
chmod 750 /var/www/html/

rm /var/www/html/index.html
echo "<html><body>Are you lost? Ok, I'll help you, you're in front of a screen...</body></html>" > /var/www/html/index.html

```

Test Nginx 

Open browser and go to page http://51.83.45.10/

 ## Define our parameters
 
 ```
 MYUSER=hephaistos
 MYPASSWORD=Gkh23hglxVd47ShG43jh3h
 MYDOMAINNAME=hello-world.hephaiscode.com
 MYEMAIL=hello-world@hephaiscode.com
 MYWEBFOLDER=WebSite
 ```
 
 ## Create user ${MYUSER}
 
 ```
useradd --shell /bin/false ${MYUSER}
echo ${MYUSER}:${MYPASSWORD} | chpasswd
mkdir /home/${MYUSER}
chown root /home/${MYUSER}
chmod go-w /home/${MYUSER}

```

Add directory

```
mkdir /home/${MYUSER}/

mkdir /home/${MYUSER}/${MYWEBFOLDER}_NOSSL

rm /home/${MYUSER}/${MYWEBFOLDER}_NOSSL/index.html
echo '<html><body>Hello World!</body></html>' >> /home/${MYUSER}/${MYWEBFOLDER}_NOSSL/index.html

chown -R ${MYUSER}:www-data /home/${MYUSER}/${MYWEBFOLDER}_NOSSL
chmod -R 750 /home/${MYUSER}/${MYWEBFOLDER}_NOSSL

```

## Install Domain Name

Create the host parameters for Apache and our domains **hello-world.hephaiscode.com**

```
MYAPACHECONFNOSSL=/etc/nginx/sites-available/${MYDOMAINNAME}_NOSSL
rm ${MYAPACHECONFNOSSL}
echo 'server {' >> ${MYAPACHECONFNOSSL}
echo " listen 80;" >> ${MYAPACHECONFNOSSL}
echo " listen [::]:80;" >> ${MYAPACHECONFNOSSL}
echo " root /home/${MYUSER}/${MYWEBFOLDER}_NOSSL;" >> ${MYAPACHECONFNOSSL}
echo " index index.html index.htm index.nginx-debian.html;" >> ${MYAPACHECONFNOSSL}
echo " server_name ${MYDOMAINNAME};" >> ${MYAPACHECONFNOSSL}
echo ' location / {' >> ${MYAPACHECONFNOSSL}
echo '  try_files $uri $uri/ =404;' >> ${MYAPACHECONFNOSSL}
echo ' }' >> ${MYAPACHECONFNOSSL}
echo '}' >> ${MYAPACHECONFNOSSL}

ln -s /etc/nginx/sites-available/${MYDOMAINNAME}_NOSSL /etc/nginx/sites-enabled/

systemctl restart nginx

```

## Hello World Test

Open browser and go to page http://hello-world.hephaiscode.com 

## Hello World Success

![Success](https://img.shields.io/badge/Hello%20World-OK-Green.svg)
