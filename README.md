# Project 5: Linux Server Configuration

## Server Details
Server IP Address: 3.95.119.29

SSH Port: 2200

Server URL: [3.95.119.29.xip.io](http://3.95.119.29.xip.io)

## Software Installed
* Ubuntu 16.04 LTS
* Apache2
* libapache2-mod-wsgi
* PostsgreSQL
* Git
* python-pip
* python-psycopg2
* python-sqlalchemy
* flask
* requests
* httplib2
* oauth2client
* DateTime

## Configuration
1. Created linux web server and downloaded main account key file.

Went to [Amazon Lightsail Site](http://lightsail.aws.amazon.com/) and created a linux web server running Ubuntu 16.04 LTS.

2. Logged into web server using ssh.
```
ssh ubuntu@3.95.119.29 -p 22 -i ~/.ssh/key.pem
```
3. Update all packages
```
sudo apt-get update
sudo apt-get full-upgrade
```
4. Configure server to refuse remote root login and change the ssh port from the default 22 to 2200.
```
sudo nano /etc/ssh/sshd_config
```
```
# What ports, IPs and protocols we listen for
Port 2200

# Authentication:
PermitRootLogin: no
```
5. Configure netfilter firewall to allow connections only on 2200 (ssh), 123 (ntp), and 80 (http).
```
sudo ufw status
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200/tcp
sudo ufw allow ntp
sudo ufw allow www
sudo ufw enable
```
6. Added user account for myself `zamerman` and gave it sudo privileges.
```
sudo adduser zamerman
sudo touch /etc/sudoers.d/zamerman
```
```
zamerman ALL = (ALL) NOPASSWD:ALL
```
```
sudo -i -u zamerman
cd /home/zamerman
mkdir .ssh
touch .ssh/authorized_keys
chmod 700 .ssh
chmod 644 .ssh/authorized_keys
exit
```

7. Force key based authentication.
```
sudo nano /etc/ssh/sshd_config
```
```
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication no
```
```
sudo service ssh restart
```

I can now login as zamerman with:

```
ssh zamerman@3.95.119.29 -p 2200 -i ~/.ssh/zamerman_rsa
```

8. Added grader account gave it sudo priveleges and generated key pair.

Same as above just with grader user.

9. Checked timezone.
```
date
```

If timezone was not utc change it with.

```
sudo dpkg-reconfigure tzdata
```
7. Installed apache2, wsgi, python-pip, and postgresql database.
```
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi
sudo apt-get install python-pip
sudo apt-get install postgresql postgresql-contrib
```
8. Configured apache2 to run flask with wsgi and linked to a Hello World flask application.
```
sudo nano /etc/apache2/sites-available/item_catalog.conf
sudo a2ensite item_catalog.conf
sudo service apache2restart
```
9. Experimented with postgresql and added in zamerman and catalog roles and databases.
``` 
sudo -i -u postgres
psql
create role zamerman with login
create role catalog with login
alter role catalog with createdb
\password catalog
\q
createdb zamerman
createdb catalog --owner=catalog
```
10. Checked to make sure that no remote logins to the database were allowed.
```
sudo nano /etc/postgresql/9.3/main/pg_hba.conf
```
11. Installed git and used git to pull application files
```
sudo apt-get install git
cd /var/www
sudo git pull https://github.com/zamerman/catalog.git
```
12. Used pip to install flask and other python libraries.
```
sudo pip install flask
sudo pip install oauth2client
sudo pip install httplib2
sudo pip install DateTime
sudo pip install requests
```
13. Went back and forth between apache2 error files and web page to get the application working.
```
sudo nano /var/log/apache2/error.log
sudo nano /var/www/catalog/item_catalog.py
sudo nano /var/www/catalog/item_catalog.wsgi
sudo nano /var/www/catalog/catalog_setup.py
```
14. Registered the domain with google to enable oauth2.

[Google Verification Page](http://3.95.119.29.xip.io/googlef533f837b55dfef4.html/)

## Third-Party Resources

[How To Install and Use PostgreSQL on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-16-04)

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#lists)

[Ubuntu Server Guide](https://help.ubuntu.com/16.04/serverguide/serverguide.pdf)

[Apache HTTP Server Project](https://httpd.apache.org/)

[Using PostgreSQL with SQLAlchemy](https://www.compose.com/articles/using-postgresql-through-sqlalchemy/)
