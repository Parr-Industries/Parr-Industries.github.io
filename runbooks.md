# Server Crash
In the case of a complete server crash We have weekly snapshots of the server that are easily restored via the GCP console, they can be found [here](https://console.cloud.google.com/compute/snapshots?authuser=1&project=parrinc-191702).

##### Steps to restore from snapshot
1. Navigate to the [snapshots page](https://console.cloud.google.com/compute/snapshots?authuser=1&project=parrinc-191702)
2. Click on the snapshot you want to restore from (pay attention to the snapshot date)
3. At the top of the page click on the `CREATE INSTANCE` button 
4. Name the new instance appropriately and select the configuration based on the values [here](/#!server_config.md#Server)
5. Once the new instance is up take note of the **EXTERNAL** IP address and [update the DNS record accordingly](/#!software.md#DNS)
6. At this point the server should be restored

##### Database backups
Once the server is restored it may be necessary to restore the database. We store database backups on the server at `/var/www/html/mysql_backups` and in a GCP bucket that can be found [here](https://console.cloud.google.com/storage/browser/parr2_mysql_backups;tab=objects?forceOnBucketsSortingFiltering=true&authuser=1&hl=en&project=parrinc-191702&prefix=&forceOnObjectsSortingFiltering=false). To restore the database use the following commands:

1. SSH to the compute instance
2. Login as emory: `sudo su emory`
3. Locate the backup you want to use
4. `mysql parr2_tngen < BACKUPFILE.sql` OR `mysql parr2 < BACKUPFILE.sql` depending on which database you need to backup.

# Apache Crash - API Problems
---
In most cases Apache can be restored by simply running `sudo service apache2 restart`. If that does not work you can look in `/var/log/apache2/error.log` and/or `/var/log/syslog` for more information. It might also be useful to run `sudo service apache2 status`. In some cases Apache might have failed to start after a certificate renewal, see the [Certbot](/#!software.md#Certbot) section for more information. All APIs and webpages are managed using Apache, if an API is having issues it may be worthwhile to restart Apache (`sudo service apache2 restart`).

# MySQL Crash
---
Logs for MySQL are found in `/var/log/mysql`, utilize the logs and/or `sudo service mysql status` to determine why MySQL has crashed. Sometimes it can be restored using `sudo service mysql restart`. This can take a few minutes so don't panic.

# Low Disk Space
---
If the server runs out of disk space things stop working. To figure out how much disk space the server is using you can run `df -h`. An example output is below:
```
emory@parr2-210329:/var/www/html/wiki$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            7.4G     0  7.4G   0% /dev
tmpfs           1.5G  161M  1.4G  11% /run
/dev/sda1        74G   49G   23G  69% /
tmpfs           7.4G     0  7.4G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           7.4G     0  7.4G   0% /sys/fs/cgroup
tmpfs           1.5G     0  1.5G   0% /run/user/1001
```

The important partition to look at is `/dev/sda1`. If this indicates that the disk is full you can utilize `sudo du -h --max-depth=1` to locate what is using the disk. I suggest starting in `/` (`cd /`) and then running `sudo du -h --max-depth=1` an example output is below:
```
emory@parr2-210329:/$ sudo du -h --max-depth=1
35M	./etc
111M	./boot
4.0K	./mnt
125M	./root
34G	./var
16K	./opt
12K	./srv
4.0K	./media
91M	./tmp
9.9G	./home
882M	./lib
0	./dev
0	./proc
4.0K	./lib64
16K	./lost+found
0	./sys
9.2M	./sbin
153M	./run
9.6M	./bin
3.3G	./usr
48G	.
```

Once you locate which directory is using the most space you can navigate there and rerun `sudo du -h --max-depth=1`. Repeate this until you locate the source.
