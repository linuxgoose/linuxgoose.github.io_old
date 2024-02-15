---
layout: post
title: How to Back Up Your Ghost Install (Writing Sample)
subtitle: A tutorial on how to back up your Ghost blogging platform install.
tags: [how-to,ghost,ubuntu,writing-sample,bash]
comments: true
author: Jordan Robinson
---

In this tutorial we will show you how to back up your Ghost Blogging Platform installation and database. We will be using a VPS with Ubuntu 18.04 installed.

Before we start, if you are simply looking for a simple Export/Import option for your Ghost site, you are in luck! Ghost provides this capability within the admin interface. This can be found under the "Labs" menu item and then within the "Migration Options" section of that page. Alternatively, you can go to **"https://exampleghostsite.com/ghost/#/settings/labs/"** (substitute the URL where applicable for your Ghost site).

Now, for a more in depth back up tutorial please follow along below.

## Here is what you will need

- A Ghost Install on Ubuntu 18.04
- Sudo privileges

## Back Up Your Ghost Install Folder

For this tutorial we will be assuming that your Ghost installation is located at /var/www/html/.

The first step is to ensure that your Ghost install is stopped. This requires us to go inside the Ghost install folder and use the Ghost CLI tool. Issue the following commands to do so:

```bash
cd /var/www/html/ghost/
ghost stop
```

Now, we will need to navigate to where the Ghost install folder is located and make a copy of the folder. Issue  the following commands to do so:

```bash
cd /var/www/html/
sudo cp -r ./ghost ./ghost_backup
``

**Optional Step:**

Now, move the backup folder to your backup location with the following command. Substitute "BACKUP_LOCATION_PATH" with your actual backup location.

```bash
sudo mv ghost_backup /BACKUP_LOCATION_PATH/
```

And just like that, your Ghost install folder is backed up. Onto the database.

## Back Up Your Ghost Database

For this tutorial we will be assuming that your Ghost database is named "ghost_prod". Ammend the commands as needed if your database name is different.

Navigate to your backup location (as used above).

```bash
cd /BACKUP_LOCATION_PATH/
```

Inside your backup location issue the following command to perform a database dump.

```bash
sudo mysqldump --single-transaction -h localhost -p ghost_prod > ghost_proddb_`date +"%Y%m%d"`.bak
```

You have now successfully backed up your Ghost database.

## Start Ghost
Since Ghost is now backed up you will need to start Ghost again for your site to be back up and running. Issue the following commands to do so:

```bash
cd /var/www/html/ghost/
ghost start
```

And that is it! Everything is backed up and ready to go. We recommend to take regular back ups of your site to ensure minimal downtime should anything go wonky. Whether this is just using the Export/Import option within Ghost or to perform full backups of the install folder and the database, it is an important step.