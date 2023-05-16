## Software

### Apache2
All websites are served via [Apache2](https://httpd.apache.org/). For serving the various [Flask APIs](https://flask.palletsprojects.com/en/2.3.x/) we depend on the `mod_wsgi` plugin for apache. Configuration can be found in `/etc/apache2` with individual site configurations found in `/etc/apache2/sites-enabled`. The configuration can also be found in [GitHub](https://github.com/Parr-Industries/system).

#### Adding a new site
To add a new site you must add a new site configuration file in `/etc/apache2/sites-available`. It may be useful to just copy an existing one and edit it to meet your needs. The configuration files are owned by `root` so `sudo` will be required to copy/edit them. Once you are satisfied you can enable the site using `sudo a2ensite <site_config_file>`, this creates a symlink in `/etc/apache2/sites-enabled`. Once this is done you will need to reload apache using `sudo service apache2 reload`.

#### Logging
* Access Logging: `/var/log/apache2/access.log`
* Error Logging: `/var/log/apache2/error.log`

#### Useful commands
* Restart apache: `sudo service apache2 restart`
* Enable a new site: `sudo a2ensite <site_config_file>` 
* Disable a site: `sudo a2dissite <site_config_file>`

### Certbot
For SSL certficates we utilize [LetsEncrypt](https://letsencrypt.org/) certificates managed by [certbot](https://certbot.eff.org/). Once the certificates are created they are autorenewed. To create a new certificate run `sudo certbot certonly --authenticator standalone --pre-hook "service apache2 stop" --post-hook "service apache2 start"` and follow the on-screen prompts.

### MySQL
For our database we use MySQL version 5.7.25. The configuration is located at `/etc/mysql` but can also be found in [GitHub](https://github.com/Parr-Industries/system).

### PHP
We still utilize PHP for several of our pages. We use version 7.3.31-1. The configuration is located at `/etc/php/7.3/apache2/` but can also be found in [GitHub](https://github.com/Parr-Industries/system).

### Cron
We have a variety of cron scripts that run for monitoring, alerting, reporting, etc... They are located in `/etc/cron.d` but can also be found in [GitHub](https://github.com/Parr-Industries/system).

Some important ones:

* `/etc/cron.d/mysql_backup` - this keeps a daily, hourly, and even sub-hourly backup of the database
* `/etc/cron.d/apache_alert` - alerts when apache2 stops running, if this alerts then the website is down
* `/etc/cron.d/disk_space_alert` - alerts when the disk is below 10% free

### DNS
For DNS we use [Google Domains](https://domains.google.com). Each site should have an `A record` that points to the external IP address of the server and a `CNAME record` for `www` that points to the domain name (ie. dpsite.net). Apache handles all routing once the request makes it to our server, via the `A record`.
