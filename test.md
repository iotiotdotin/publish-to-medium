## Content Management System on BrainyPi

**Description:** Joomla is a popular open-source content management system that allows you to publish your web content easily. It is very flexible as it helps to manage different types of web content.

**Image:**

![test-img](https://github.com/iotiotdotin/publish-to-medium/raw/main/syncthing.png)

**Steps to Install:**


1. Just copy and paste the following lines into a terminal:

* Setting up NGINX on the Brainy Pi.

```
sudo apt update
sudo apt upgrade

sudo apt remove apache2

sudo apt install nginx

sudo systemctl start nginx
```

* Configuring NGINX for PHP.

```
sudo apt install php7.4-fpm php7.4-mbstring php7.4-mysql php7.4-curl php7.4-gd php7.4-curl php7.4-zip php7.4-xml -y

sudo nano /etc/nginx/sites-enabled/default
```

* Find and Replace following lines

Find

```
index index.html index.htm;
```

Replace With

```
index index.php index.html index.htm;
```

Find

```
#location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php5-cgi alone:
        #       fastcgi_pass 127.0.0.1:9000;
        #       # With php5-fpm:
        #       fastcgi_pass unix:/var/run/php5-fpm.sock;
        #}
```

Replace With

```
location ~ \.php$ {
               include snippets/fastcgi-php.conf;
               fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        }
```

* Once that is all done, we can save and exit by pressing CTRL + X then Y and lastly ENTER.

```
sudo systemctl reload nginx
```

* To this file, add the following line of code.

* Once that is all done, we can save and exit by pressing CTRL + X then Y and lastly ENTER.

2. To set up MYSQL on Brainy Pi:

* Run the following commands.

```
sudo apt update
sudo apt upgrade

sudo apt install mariadb-server
```

* It will ask several questions, answer _**Y**_ or _**n**_ accordingly.

3. To install Joomla on the Brainy Pi:

* Run

```
sudo apt install php7.4-intl

sudo nano /etc/nginx/sites-available/joomla.conf
```

* Copy and paste the following text into this file.

```shell
server {
    listen 80;
    listen [::]:80;

    root /var/www/joomla;

    index index.php index.html index.htm;
    server_name _;

    client_max_body_size 100M;
    autoindex off;
    
    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    # deny running scripts inside writable directories
    location ~* /(images|cache|media|logs|tmp)/.*.(php|pl|py|jsp|asp|sh|cgi)$ {
      return 403;
      error_page 403 /403_error.html;
    }

    location ~ .php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # caching of files 
    location ~* \.(ico|pdf|flv)$ {
            expires 1y;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|swf|xml|txt)$ {
            expires 14d;
    }
}
```

```shell
sudo ln -s /etc/nginx/sites-available/joomla.conf /etc/nginx/sites-enabled/joomla.conf

sudo rm /etc/nginx/sites-enabled/default

sudo systemctl reload nginx

sudo mysql -u root -p
```

* Enter the following lines

CREATE USER 'joomlausr'@'localhost' IDENTIFIED BY '\[PASSWORD\]';

GRANT ALL PRIVILEGES ON joomladb.\* TO 'joomlausr'@'localhost';

FLUSH PRIVILEGES;

quit;

* Run the commands

```shell
sudo mkdir -p /var/www/joomla

cd /var/www/joomla

sudo wget https://downloads.joomla.org/cms/joomla4/4-0-3/Joomla_4-0-3-Stable-Full_Package.tar.gz

sudo tar -xvf Joomla_4-0-3-Stable-Full_Package.tar.gz

sudo rm Joomla_4-0-3-Stable-Full_Package.tar.gz

sudo chown -R www-data:www-data /var/www/joomla*
```
4. Installation will be completed.

**Useful for:** Publishing web contents, flexible, ease of use.

**Link to original project:** https://github.com/joomla

**Link to Youtube Video:** <!-- Link to the Youtube video. -->
