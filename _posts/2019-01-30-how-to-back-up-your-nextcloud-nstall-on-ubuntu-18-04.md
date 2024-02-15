---
layout: post
title: How to Back Up Your Nextcloud Install On Ubuntu 18.04 (Writing Sample)
subtitle: A tutorial on how to back up your nextcloud install on Ubuntu 18.04.
tags: [how-to,nextcloud,ubuntu,writing-sample,bash]
comments: true
author: Jordan Robinson
---

As of right now there is no true way to back up your Nextcloud instance through the web app. If you have installed your Nextcloud instance through a tool like Softaculous, this does not apply to you as there is a much more simple way to perform a back up. There will be a separate tutorial to show you how to perform a back up within Softaculous.

For this tutorial we will assume that your Nextcloud install is located within */var/www/nextcloud*. If your installation path is different, please adjust the below commands as necessary.

## Here is what you will need
- A Nextcloud Install on Ubuntu 18.04
- Sudo privileges

## Turn Maintenance Mode On

In order to safely perform a back up you will need to put your Nextcloud install into maitenance mode. To do so, perform these commands:

```bash
cd /var/www/nextcloud
sudo -u www-data php occ maintenance:mode --on
```

## Back Up The Folders & Files

Next what we will be doing is taking a copy of the entire Nextcloud folder with rsync. We recommend placing the backup in a separate location from the original install folder. Use the following commands to perform the back up:

```bash
cd /var/www/
sudo rsync -Aavx nextcloud/ /SEPARATE_LOCATION/nextcloud_backup_`date + "%Y%m%d"`/
```

***Note:*** *Replace SEPARATE_LOCATION with the path to your back up location.*

## Back Up the Database (Optional)

Next, we will perform a back up your Nextcloud database. If you are using MySQL or MariaDB, use this command:

```bash
sudo mysqldump --single-transaction -h localhost -u YOUR_USER -p nextcloud > nextcloud-db-backup_`date + "%Y%m%d"`.bak
```

If your database location is **not** on the same machine as your Nextcloud install replace "localhost" with the location. Replace "YOUR_USER" with the user for your MySQL user login.

***Note:*** *The above command will save the database dump in the current directory.*

## Turn Maintenance Mode Off

Now that you have your database and files backed up, you will need to take your Nextcloud install out of maintenance. Issue the below command to do so:

```bash
cd /var/www/nextcloud
sudo -u www-data php occ maintenance:mode --off
```

And just like that your Nextcloud install is all backed up!