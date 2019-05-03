# Project 5: Linux Server Configuration
Server IP Address: 3.95.119.29
SSH Port: 2200
Server URL: 3.95.119.29.xip.io

1. Updated all packages with `sudo apt-get full-upgrade`.
2. Configured `/etc/ssh/sshd_config` getting rid of remote root login and changing ssh port to 2200.
3. Configured `ufw` to allow connections only on 2200 (ssh), 123 (ntp), and 80 (http).
4. Added user account for myself `zamerman` and gave it sudo privileges (partially as an experiment but later on I always used it).
5. Added grader account gave it sudo priveleges and generated key pair.
6. Checked timezone.
7. Installed apache2, wsgi, python-pip, and postgresql database.
8. Configured apache2 to run flask with wsgi and linked to a Hello World flask application.
9. Experimented with postgresql and added in zamerman and catalog users.
10. Altered catalog user priveleges to login and created.
11. Used git to pull in application files.
12. Used pip to install flask and other python libraries.
13. Went back and forth between apache2 error files and web page to get the application working.
14. Registered the domain with google to enable oauth2.
15. Altered apache files to prevent accessing .git files.
