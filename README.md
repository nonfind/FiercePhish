# FirePhish


FirePhish is a CMS for phishing exercises. It allows you to track separate phishing campaigns, schedule sending of emails, and much more. The features will continue to be expanded and will include website spoofing, click tracking, and extensive notification options.

## Installation

Installation is quite simple. Follow the instructions below to install it.  An installer will be created soon to perform these actions for you.

#### System Requirements
 * Linux
 * PHP >= 5.5.9
 * OpenSSL PHP Extension
 * PDO PHP Extension
 * Mbstring PHP Extension
 * Tokenizer PHP Extension
 * Rewrite PHP Extension

#### Automated Installation
The installer functions for the following distributions:
 * Ubuntu 14.04
 * Ubuntu 16.04
 * Ubuntu 16.10

**If you would like an additional distribution to be supported, please open a Github Issue and I will add support for it.**

*Easiest installation method*

Run the following commands and answer the questions when prompted
```
wget https://github.com/Raikia/FirePhish/installer.sh
chmod +x installer.sh
./installer.sh
```

*Another easy installation method*

On first run, this method will create a configuration file in ~/firephish.config. Fill out the information in the configuration file and run the same command again for an unattended install (~/firephish.config will be deleted automatically after installation)
```
curl https://github.com/Raikia/FirePhish/installer.sh | bash
```

#### Manual Installation Commands (assuming Ubuntu 16.04)

*Install Apache/PHP/Mysql*

```
apt-get install apache2 php mysql-server php-mysql libapache2-mod-php php-mcrypt php-mbstring phpunit composer npm unzip
npm install -g bower
ln -s /usr/bin/nodejs /usr/bin/node
```

*Download FirePhish*

```
git clone https://github.com/Raikia/FirePhish.git /var/www/firephish
chown -R www-data:www-data /var/www/firephish
```

*Configure Apache*

Edit "/etc/apache2/sites-enabled/000-default.conf" to be:

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/firephish/public
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

*Install PHP extensions and restart server*

Enable rewrite extension:
```
a2enmod rewrite
```
In "/etc/apache2/apache2.conf", find:
```
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```
Change to (note the "AllowOverride" line):
```
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
```
Restart apache service:

```
service apache restart
```

Install FirePhish:

```
composer install
bower install --allow-root
mysql -u root -p -e 'create database firephish'
```

Configure FirePhish:

```
cd /var/www/firephish
cp .env.example .env
chown www-data:www-data .env
```

Edit "/var/www/firephish/.env" using your favorite text editor (vim/emacs/nano/etc)

Make sure you set:

 * APP_URL
 * DB_HOST
 * DB_DATABASE
 * DB_USERNAME
 * DB_PASSWORD

If you don't know how to configure a MySQL instance, google it :-)

*Generate Application Encryption Key*

```
php artisan key:generate
```

*Create MySQL structure*
```
php artisan migrate
```

*Create first user account*
```
php artisan fp:createuser
```

*Run the queue processor in the background (Supervisord is awesome for this)*
```
php /var/www/firephish/artisan queue:listen --queue=high,medium,low,default
```

Done!  Browse to your FirePhish website, like http://10.10.10.10/. You can change the subdirectory you want it to appear like later (under Settings --> Configuration).

**You still need to configure the SMTP server and/or mailgun to actually send emails. Once you are logged in, browse to "Settings" --> "Configuration" and edit the mail settings.**


## Roadmap

Features coming soon:
* File hoster with notifications when someone browses to the file being hosted (both email and text notifications)
* Site spoofer - Host entire fake websites that allow gathering of credentials or prompting a download, complete with notifications and click/browsing metrics.
* Inbox for email responses.
* Much more!  See GitHub Issues for enhancements planned

## Troubleshooting

If you discover any bugs with FirePhish, please make a GitHub Issue and it will be prompty addressed.  If you have feature requests, make a GitHub issue and I will do my best at adding it.

## Screenshots

Coming soon
