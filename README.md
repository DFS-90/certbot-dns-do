# Domain-Offensive DNS Authenticator plugin for Certbot

## Why this fork?

Maybe I'm not the only one who struggled finding out how to use this plugin originally created by georgeto.
Thus, I forked it and wrote this readme.
@georgeto: Thanks for this great plugin! 👍


## General

This plugin automates the process of completing a ``dns-01`` challenge by creating, and subsequently removing, TXT records using the DomainOffensive (do.de) API.


## Credentials

Before being able to this plugin, you need to obtain an API token from your DomainOffensive account:

https://my.do.de/settings/domains/general

Click on the button next to "Let's Encrypt API-Token" and write down the API key that is shown in the newly opened window.


## How to install

I finally got it working and wanted to share my findings with you.
All steps described have been tested on Debian 11 as root.


Install Certbot, git and nano (text editor):
```
apt update
apt install certbot git nano
```

Create a folder to clone this plugin to, then clone the plugin:
```
mkdir /usr/src && cd /usr/src
git clone https://github.com/georgeto/certbot-dns-do.git
```

Access the plugin folder and install it:
```
cd /usr/src/certbot-dns-do
python3 setup.py install
```

Check if the plugin has been installed successfully:
```
certbot plugins
```
-> should show a plugin called dns-do:
```
    dns-do
    Description: Obtain certificates using a DNS TXT record (if you are using
    Domain-Offensive for DNS).
    Interfaces: IAuthenticator, IPlugin
    Entry point: dns-do = certbot_dns_do.dns_do:Authenticator
```

Create a credentials file and fill it with your DomainOffensive Letsencrypt token:
```
nano /etc/certbot_credentials.ini
```

Write the following into the file:
```
# DomainOffensive API credentials used by Certbot
dns_do_api_token = <TOKEN>
```

Protect the credentials file:
```
chmod 600 /etc/certbot_credentials.ini
```

Obtain a certificate - try a dry run first:
```
certbot certonly --authenticator dns-do --dns-do-credentials /etc/certbot_credentials.ini --dry-run --agree-tos -m <MAIL ADDRESS> -d <DOMAIN>
```

If the dry run worked, obtain a certificate:
```
certbot certonly --authenticator dns-do --dns-do-credentials /etc/certbot_credentials.ini --agree-tos -m <MAIL ADDRESS> -d <DOMAIN>
```
