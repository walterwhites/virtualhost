# virtualhost
configuration apache virtualhosts

Note By default, Apache configuration on Mac OS X serves files from /Library/WebServer/Documents for localhost

## Table of Contents

* [Conf](#conf)
* [Debug](#debug)

## Edit apache conf

1) Make you super user:
```
sudo su
```

2) Make you super user:
```
nano /etc/apache2/httpd.conf
```

3) Add this line:
```
Include /private/etc/apache2/vhosts/*.conf
```

3) Add this line to include all files ending in .conf in the /private/etc/apache2/vhosts/ directory:
```
Include /private/etc/apache2/vhosts/*.conf
```

4) Do:
```
mkdir /etc/apache2/vhosts
cd /etc/apache2/vhosts
```

5) Create virtual host configuration file:
```
nano _default.conf
```

6) Add (for exemple):
```
<VirtualHost *:80>
    DocumentRoot "/Library/WebServer/Documents"
</VirtualHost>
```

7) Create Vhost configuration file (with the that you want):
```
exemple.site.net.conf
```

8) Add on it (replace by your username and your dir project):
```
<VirtualHost *:80>
        DocumentRoot "/Users/(my_username)/Documents/workspace/(my_dir_project)/htdocs"
        ServerName exemple.site.local
        ErrorLog "/private/var/log/apache2/exemple.site.local-error_log"
        CustomLog "/private/var/log/apache2/exemple.site.local-access_log" common

        <Directory "/Users/(my_username)/Documents/workspace/(my_dir_project)/htdocs">
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

9) Run
```
apachectl restart
apachectl configtest
```

10) edit etc/hosts
```
nano /etc/hosts
```

11) add:
```
127.0.0.1       exemple.site.local
```

12) Clear local DNS cache
```
dscacheutil -flushcache
```

13) After you will can access to: http://exemple.site.local for local development.

Below, the command to run from /etc/apache2 to change default dir "Library/WebServer/Documents" by "/Users/(your_username)/workspace/Sites" (be careful, it change in all .conf files) :
```
LC_CTYPE=C && LANG=C && find * -type f -exec sed -i "" "s#/Library/WebServer/Documents#/Users/(your_username)/workspace/Sites#g" {} +
```

## Debug
Sometimes we should kill httpd to reload server:
```
sudo apachectl stop  
sudo killall httpd
sudo apachectl start  
sudo dscacheutil -flushcache
```

To fix Error 403:

1) change Rights on folder of you (To make readable by Apache)
```
chmod a+rx  /var/www/your_folder
chmod a+r /var/www/votre-dossier/your_folder
```

2) add index.html file
```
cd /var/www/your_folder
nano index.html
Hello word
```

3) add Options +Indexes on .htaccess
```
cd /var/www/your_folder
nano .htaccess
Options +Indexes
```
