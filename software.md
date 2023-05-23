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

### Python and APIs
For various tooling, scripts, as well as our APIs we use Python. We run version 3.7.3. The APIs are served using [WSGI](https://wsgi.readthedocs.io/en/latest/what.html) via the [mod_wsgi](https://modwsgi.readthedocs.io/en/master/) module for Apache. The APIs are written using the [Flask](https://flask.palletsprojects.com/en/2.3.x/) framework. Below are the installed packages and versions. These can also be found in [GitHub](https://github.com/Parr-Industries/system) and at `/home/emory/requirements.txt` and can be reinstalled by running `pip3 install -r /home/emory/requirements.txt`. Config files for the APIs can be found in `/etc` and example can also be found in [GitHub](https://github.com/Parr-Industries/system). 
```
2to3==1.0
aiohttp==3.6.2
apiclient==1.0.4
arrow==0.15.7
async-timeout==3.0.1
attrs==19.3.0
beautifulsoup4==4.9.3
cachetools==4.1.0
certifi==2020.4.5.2
chardet==3.0.4
click==7.1.2
flake8==3.8.3
Flask==1.1.2
Flask-Cors==3.0.8
Flask-Excel==0.0.7
Flask-JWT==0.3.2
Flask-JWT-Extended==3.24.1
gcloud==0.18.3
google-api-core==1.21.0
google-api-python-client==1.11.0
google-auth==1.18.0
google-auth-httplib2==0.0.3
google-auth-oauthlib==0.4.2
google-cloud==0.34.0
google-cloud-core==2.3.1
google-cloud-pubsub==2.1.0
google-cloud-storage==2.4.0
google-crc32c==1.3.0
google-resumable-media==2.3.3
googleapis-common-protos==1.52.0
googlemaps==4.4.1
grpc-google-iam-v1==0.12.3
grpcio==1.32.0
gviz-api==1.9.0
httplib2==0.18.1
idna==2.9
importlib-metadata==1.7.0
itsdangerous==1.1.0
Jinja2==2.11.2
keytree==1.1.0
libcst==0.3.13
lml==0.0.9
marko==1.1.0
MarkupSafe==1.1.1
mccabe==0.6.1
MonthDelta==0.9.1
multidict==4.7.6
mygeotab==0.8.5
mypy-extensions==0.4.3
mysqlclient==1.4.6
oauth2client==4.1.3
oauthclient==1.0.3
oauthlib==3.1.0
Pillow==7.1.2
polyline==1.4.0
proto-plus==1.11.0
protobuf==3.12.2
pyasn1==0.4.8
pyasn1-modules==0.2.8
pycodestyle==2.6.0
pyexcel==0.6.2
pyexcel-io==0.5.20
pyexcel-webio==0.1.4
pyflakes==2.2.0
PyJWT==1.7.1
pyparsing==3.0.9
python-dateutil==2.8.1
python-magic==0.4.18
python-rapidjson==0.9.1
pytz==2020.1
PyYAML==5.3.1
redis==3.5.3
requests==2.24.0
requests-oauthlib==1.3.0
rsa==4.6
Shapely==1.7.0
six==1.15.0
soupsieve==2.0.1
swagger-client==1.0.0
tabulate==0.8.9
texttable==1.6.2
typing-extensions==3.7.4.3
typing-inspect==0.6.0
uritemplate==3.0.1
urllib3==1.25.9
Werkzeug==1.0.1
yarl==1.4.2
zipp==3.1.0
```
