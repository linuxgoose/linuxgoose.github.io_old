---
layout: post
title: How to Install and Set up a Matrix-Synapse server on Ubuntu 18.04 (Writing Sample)
subtitle: A tutorial on how to install and set up a Matrix-Synapse server on Ubuntu 18.04.
tags: [how-to,matrix,synapse,ubuntu,writing-sample]
comments: true
author: Jordan Robinson
---

## What is Matrix Synapse?

You may be wondering "what is Matrix Synapse and why should I know about it?".

First, Matrix and Synapse are two different things. **Synapse** is a reference homeserver implementation written in Python/Twisted by the core team over at Matrix.org.  **Matrix** is an open standard for interoperable and decentralized communication over IP. Users are able to use the open source technology for real-time communication with peers.

Matrix allows for users to connect their own homeserver to the Matrix network through federation. But, what is federation?

**Federation** is a group of smaller entities that together form a larger entity; a union of entities. Matrix allows for open federation so that anyone, anywhere, can join the Matrix ecosystem if they have their own Matrix Synapse server.

Matrix Synapse is a great alternative for applications like Slack, Discord, Rocket.chat, Skype and others.

In this tutorial we will go through the process step-by-step on the prerequisites, installation, and configuration of a Matrix Synapse server. This tutorial will be using Ubuntu 18.04 and the Apache web server as a reverse proxy. Matrix can be configured to use Nginx as well. This will be shown in a later tutorial.

## What you will need
- A VPS server with Ubuntu 18.04
- Root privileges
- A domain name or subdomain to be used for Matrix Synapse

## Install prerequisites

### Install Universe Repository

```bash
sudo add-apt-repository universe
```

### Install Apache Web Server

```bash
sudo apt install apache2
```

***Note:*** *We will be configuring Apache at a later step*

### Install Let's Encrypt (certbot)
### Add the repository

```bash
sudo add-apt-repository ppa:certbot/certbot
```

#### Install Certbot's Apache Web Server package

```bash
sudo apt install python-certbot-apache
```

#### Set up well.known

On your VPS create an 'index.html' file in the directory /.well-known/matrix/server with the following content:

```bash
cd /var/www/html/.well-known/matrix/server
sudo nano index.html
```

### Set up SRV Record
An SRV record tells other Matrix Synapse servers where they should connect to your server. The below record points them to the correct hostname and port. **Replace matrix.decibite.com with your own domain/subdomain.**

```bash 
_matrix._tcp.decibite.com. 3600 IN SRV 10 5 443 matrix.decibite.com.
```

## Install Matrix Synapse Server
Add  Matrix key

```bash
wget -qO - https://matrix.org/packages/debian/repo-key.asc | sudo apt-key add -
````

Add Matrix repository

```bash
sudo add-apt-repository https://matrix.org/packages/debian/
```

## Install Matrix Synapse

```bash
sudo apt install matrix-synapse -y
```

**Step 1:** Enter your domain (or subdomain) as your server name

**Step 2:** Choose 'No' for the anonymous data report

After the installation completes, start the service.

```bash
sudo systemctl start matrix-synapse
```

Enable the service to start at system boot.

```bash
sudo systemctl enable matrix-synapse
```

Verify that Matrix Synapse is running using the netstat command. The default configuration is to use ports '8008' (HTTP) and '8448' (HTTPS).

```bash
netstat -plntu
```
## Configuration of Matrix Synapse

Awesome! Now that you have Matrix Synapse installed, let's go ahead and configure it to work properly.

In this tutorial we will show you how to configure Matrix Synapse to run under the local IP and enable registration through the registration-secret-key.

Before you edit the default homeserver configuration file, generate your server's shared-secret-key. **Save this key somewhere safe.**

```bash
cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1
```

Now, let's edit the main homeserver configuration file. **Fair warning: YAML files are finnicky and space sensitive. When editing ensure you DO NOT change the formatting or spacing; only the values.**

```bash
cd /etc/matrix-synapse/
sudo nano homeserver.yaml
```

Change the HTTP Listener port (8008) to the local IP address of '127.0.0.1'.

Disable Matrix Synapse registration by setting 'enable_registration' to false and then uncomment 'registration_shared_secret'. Paste your shared-secret-key where 'insert_secret_key' is in the screenshot below (Tip: use CTRL + W to search/find).

***Important Note:*** *'registration_shared_secret' once set allows anyone who has the secret key to register USERS and ADMINS. Even when registration has been disabled.*

Restart your Matrix Synapse server to implement the changes

```bash
sudo systemctl restart matrix-synapse
```

## Generate your Let's Encrypt Certificates

Since we are using Apache Web Server in this tutorial use the following command to generate your SSL certificate **(replace matrix.decibite.com with your own domain/subdomain)**.

```bash
sudo certbot --apache -d matrix.decibite.com,www.matrix.decibite.com
```

***Note:*** *'www' domain is not a requirement.*

Follow the steps presented to you as required.

## Configuration of Apache Web Server

Now that the Matrix Synapse server is all set up let's go ahead and configure the Apache host file.

### Apache Modules Required
Before we do that though, we need to enable a few Apache modules.

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_balancer
sudo a2enmod ssl
sudo a2enmod headers
sudo a2enmod lbmethod_byrequests
```

### Edit the Configuration File
Next, go to the Apache Web Server directory.

```bash
cd /etc/apache2/sites-enabled/
```

Open the configuration file

```bash
sudo nano 000-default.conf
```

Edit your host file to look like the following.

```bash
<IfModule mod_ssl.c>
<VirtualHost matrix.decibite.com:8448>
        ServerName matrix.decibite.com
        ServerAlias www.matrix.decibite.com
        DocumentRoot /var/www/html/

        # Configuration of the SSL Certificate
        SSLEngine On
        Include /etc/letsencrypt/options-ssl-apache.conf
        SSLCertificateFile /etc/letsencrypt/live/matrix.decibite.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/matrix.decibite.com/privkey.pem

        <Location />
        ProxyPass http://127.0.0.1:8008/ nocanon
        ProxyPassReverse http://127.0.0.1:8008/
        </Location>
</VirtualHost>
</IfModule>

<IfModule mod_ssl.c>
<VirtualHost matrix.decibite.com:443>
        ServerName matrix.decibite.com
        ServerAlias www.matrix.decibite.com
        DocumentRoot /var/www/html/

        ProxyRequests off
        ProxyPreserveHost On
        ProxyVia full

        ProxyPass /_matrix/identity http://127.0.0.1:8090/_matrix/identity

        <Location />
        ProxyPass http://127.0.0.1:8008/ nocanon
        ProxyPassReverse  http://127.0.0.1:8008/
        </Location>

        RequestHeader set X-Forwarded-Proto "https"

        # Configuration of the SSL Certificate
        SSLEngine On
        Include /etc/letsencrypt/options-ssl-apache.conf
        SSLCertificateFile /etc/letsencrypt/live/matrix.decibite.com/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/matrix.decibite.com/privkey.pem
</VirtualHost>
</IfModule>
```

### Set up UFW Firewall

In order for all services to work properly we will need to allow a few ports. Run the following commands to allow them:

```bash
ufw allow ssh
ufw allow http
ufw allow https
```

Now enable UFW Firewall service to run at each startup.

```bash
ufw start
ufw enable
```

Check the status and verify.

```bash
ufw status
```

## Adding Users to your Matrix Synapse Server

Let's add some users and admins! To add a user use the below command:

```bash
sudo register_new_matrix_user -c /etc/matrix-synapse/homeserver.yaml https://127.0.0.1:8008
```

The system will now ask for the users username, password, and whether they should be made an admin of the server or not. After you have filled out the questions the user or admin will be successfully created.

## Test The Connection
### Test Connection to Your Matrix Synapse Server
In your web browser open your domain. In the example, this would be https://matrix.decibite.com . If it worked, you should get a success screen.

### Test Federation
There is a great tool that a contributor of the Matrix project has created to allow users to test if federation has been configured properly on their own server. Navigate to the federation tester by clicking here and entering in your domain. If all is working you should see a success screen displayed.