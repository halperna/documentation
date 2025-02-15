---
title: Setup TagSpaces Pro Web
---

## Introduction

TagSpaces Pro Web is a web based single page web application. For hosting it you need a web server able to host static content such as .html, .png, and .js files.

This tutorials explains you how to setup the "App Hosting" area located in the upper right part of the following deployment architecture diagram:

![Deployment architecture of TagSpaces Web Pro](https://www.tagspaces.org/diagrams/tagspaces-pro-web-software-architecture.png)

## Download and unzip

The package can be downloaded from the link available in the email you will receive from our payment provider after purchasing the product.

In the terminal you can use for example the **wget** utility to start the download.

The downloaded package is a ZIP archive file which can be unpacked with this command:

```bash
$ unzip tagspaces-enterprise-3.x.x.zip
```

The extracted files will be located in `./web` folder.

Change the ownership of all files in the web folder to your www user. In the following example the www user is called `www-user`, but this is not mandatory and on your system this user has probably another name.

```bash
$ chown -R www-user:www-user ./web
```

## Place the app in your web server

TagSpaces Pro Web can be installed on any modern web server offering static hosting of files like Apache or nginx.

Copy the content of `./web` to a folder which is hosted by your web server e.g. `./www`. So listing this folder should look something like this:

```bash
~/www $ ls -all
total 1476
    4096 Sep 17 10:29 .
    4096 Jan 15 20:42 ..
    4096 Sep 17 10:29 assets
    4096 Mär  6  2020 dist
    6038 Sep 17 10:29 index.html
   34520 Sep 17 10:29 EULA.txt
   51519 Sep 17 10:29 logo.svg
    1004 Sep 17 10:29 manifest.json
    4096 Sep 24  2019 node_modules
    3859 Sep 17 10:29 offline.html
     452 Sep 17 10:29 pwa.js
    2066 Sep 17 10:29 sw-config.js
    1129 Sep 17 10:29 sw.js
 1305364 Sep 16  2019 third-party.txt
```

## Securing the TagSpaces Web installation

It is highly recommended to use HTTPS for connection to your TagSpaces installation, especially if your installation is available on the Internet. In order have HTTPS connection you will need to setup a valid ssl certificate. You can use such for free from provider such as [Letsencrypt](https://letsencrypt.org/). Setting up a SSL certificate is not in the scope of this article.

:::tip
**Use static hosting**: In order to reduce the attack vectors it is recommended to configure your webserver to deliver only static content by disabling the support of PHP, Perl, ASP.Net, Java and other similar server side scripting language and frameworks
:::tip

## Setup basic access authentication

On a Apache or nginx web servers you can setup very easily a **[basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication)**, so not everybody knowing the URL of the installation can access the application. This is an optional step, which can be also exchanged with another way for securing the folder hosting the app.

### Create a .htaccess file

You can go to the folder which contains the TagSpaces web app and create a `.htaccess` file with this command:

```bash
$ touch .htaccess
```

This file should have the following content:

```
AuthType Basic
AuthName "Access to the staging site"
AuthUserFile /path/to/.htpasswd
Require valid-user
```

Replace in this file "/path/to/.htpasswd" with path on your system where you plan to place this file.

### Create a password file

In order to create file with user name and password pairs you will need the **htpasswd** command, which is part of **apache2-utils** (Debian, Ubuntu) or **httpd-tools** (RHEL, CentOS) packages. On Debian based Linux distributions you can install **apache2-utils** with the following commend:

```bash
$ sudo apt install apache2-utils
```

Create a password file with a initial user. Run the **htpasswd** utility with the **-c** flag (to create a new file), the file pathname as the first argument, and the username as the second argument:

```bash
$ sudo htpasswd -c /path/to/.htpasswd youruser
```

Press Enter and type the password for _youruser_ at the prompts.

Create optional additional user-password pairs. Omit the -c flag because the file already exists:

```bash
$ sudo htpasswd /path/to/.htpasswd yourseconduser
```

You can confirm that the file contains paired usernames and encrypted passwords:

```bash
$ cat /path/to/.htpasswd
youruser:$apr1$KkxMK/X2$AO2PMst7VdGgNCFKUTSit1
yourseconduser:$apr1$glNPbNg1$XvScMyvG6/TH6PX8jhoU.1
```

## Configure the web app

The TagSpaces web app can be configured with an external configuration file called **extconfig.js** located in the root path of the installation. With this file you can configure for exmaple a custom logo and colors or you can deliver predefined locations and tag library to your users. A documentation of all supported options in this configuration file can be found [here](/dev/external-config).

:::tip
You can use the property [**ExtSaveLocationsInBrowser**](/dev/external-config#saving-locations-in-the-browser) in order to enable the persistence of the location's configuration in the local storage of the user's browser. This is especially useful if you for security reasons do not want to place location configuration (containing access and secret keys) in the extconfig.js. Once enabled the end-user can configure location in her browser and they will be available the next time she uses the app.
:::

This **extconfig.js** is not part of the installation so it have to be created manually.

```bash
$ touch extconfig.js
```

After this command the installation folder should look like this:

```bash {11}
~/www $ ls -all
total 1476
    4096 Sep 17 10:29 .
    4096 Jan 15 20:42 ..
    4096 Sep 17 10:29 assets
    4096 Mär  6  2020 dist
    6038 Sep 17 10:29 index.html
   34520 Sep 17 10:29 EULA.txt
   51519 Sep 17 10:29 logo.svg
    1004 Sep 17 10:29 manifest.json
     695 Sep 17 10:29 extconfig.js
    4096 Sep 24  2019 node_modules
    3859 Sep 17 10:29 offline.html
     452 Sep 17 10:29 pwa.js
    2066 Sep 17 10:29 sw-config.js
    1129 Sep 17 10:29 sw.js
 1305364 Sep 16  2019 third-party.txt
```
