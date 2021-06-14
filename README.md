# install magwento 

sudo apt-get install php7.4 php7.4-fpm php7.4-mcrypt php7.4-mbstring php7.4-zip php7.4-curl php7.4-cli php7.4-mysql php7.4-imagick php7.4-gd php7.4-xsl php7.4-json php7.4-intl php7.4-dev php7.4-common libcurl4 curl -y

sudo mkdir /var/www/magento

sudo chown -R $USER:$USER /var/www/magento

sudo nano /etc/nginx/sites-available/magento


server {
    listen 80;
    server_name magento www.magento;
    root /var/www/magento;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
sudo ln -s /etc/nginx/sites-available/magento /etc/nginx/sites-enabled/

nano /var/www/magento/index.html

sudo ln -s /usr/share/phpmyadmin /var/www/magento/phpmyadmin

sudo apt install php7.4 php7.4-cli php7.4-fpm php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd  php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath php7.4-soap
sudo gedit /etc/php/7.4/fpm/php.ini

CREATE DATABASE magento2;

CREATE USER 'magentouser'@'localhost' IDENTIFIED BY '123456@Abc';

GRANT ALL ON magento.* TO 'magentouser'@'localhost' WITH GRANT OPTION;

cd /var/www/magento


sudo composer create-project --repository=https://repo.magento.com/ magento/project-community-edition magento2

php bin/magento setup:install --base-url="http://127.0.0.1/magento2/" --db-host="localhost" --db-name="magento2" --db-user="magentouser" --db-password="123456@Abc" --admin-firstname="admin" --admin-lastname="admin" --admin-email="admin@gmail.com" --admin-user="admin" --admin-password="123456@Abc" --language="en_US" --currency="USD" --timezone="America/Chicago" --use-rewrites="1" --backend-frontname="admin"

sudo systemctl restart nginx
sudo php bin/magento module:disable Magento_TwoFactorAuth

