# OwnCloud
Private cloud set up using a Rasberry Pi and an HHD of 5TB. 

I used VNC for remote connection to the Raspberry. You can download it [here](https://www.realvnc.com/).
I bought a no-ip domain + No-IP Vital Encrypt DV (For the SSL certificate). You can find out more on their [offical webpage](https://www.noip.com/)
For generating the certificate CSR, I used [OpenSSL CSR Tool](https://www.digicert.com/easy-csr/openssl.htm)
To enable access from outside my network, I used [this tutorial](https://www.makeuseof.com/access-owncloud-internet/)

# Raspberry Pi
## How to Install OwnCloud on Raspberry Pi 3
*5 years ago by Shahriar Shovon*

OwnCloud is a self-hosted file-sharing server. It has a nice-looking web-based UI and has apps for Linux, Windows, macOS, Android and iPhone. In this article, I am going to show you how to install OwnCloud on Raspberry Pi 3. So, let’s get started.

## Things You Need:
To successfully install OwnCloud on Raspberry Pi 3 using this article, you need,

- A Raspberry Pi 3 single-board computer.
- A microSD card of at least 8 GB or more.
- Network connectivity on Raspberry Pi.

## Installing Raspbian on Raspberry Pi:
You must have Raspbian OS installed on your Raspberry Pi 3 in order to install OwnCloud on Raspberry Pi.

I’ve written a dedicated article on installing Raspbian OS on Raspberry Pi which you can read at [this link](https://linuxhint.com/install_raspbian_raspberry_pi/). I hope it will help. If you have any questions, feel free to ask at [this support link](https://support.linuxhint.com/).

## Connecting Raspberry Pi to the Internet:
You can connect one end of your LAN cable (CAT5E or CAT6) to your Router or Switch and the other end to your Raspberry Pi to get internet connectivity easily.

You can use Wifi on your Raspberry Pi as well. I’ve written a dedicated article on that which you can read at [this link](https://linuxhint.com/rasperberry_pi_wifi_wpa_supplicant/).

## Connecting to Raspberry Pi Remotely:
Once you have Raspbian installed and configured, can connect to your Raspberry Pi using SSH.

To do that, run the following command from your laptop or desktop.

$ ssh pi@IP_ADDR


*Note: Here, IP_ADDR is the IP address of your Raspberry Pi.*

If you see this message, just type in yes and press <Enter>.

Now, type in the password of your Raspberry Pi and press <Enter>. The default password is raspberry.

## Adding OwnCloud Package Repository:
OwnCloud is not available in the official package repository of Raspbian. But you can easily add the official OwnCloud package repository on Raspbian and install OwnCloud.

First, download the GPG key of the OwnCloud package repository with the following command:

$ wget -nv https://download.owncloud.org/download/repositories/production/ Debian_9.0/Release.key -O Release.key


*The GPG key should be downloaded.*

Now, add the GPG key to the APT package manager with the following command:

$ sudo apt-key add - < Release.key


*The GPG key should be added.*

Now, run the following command to add the official OwnCloud package repository to Raspbian:

$ echo 'deb http://download.owncloud.org/download/repositories/production/Debian_9.0/ /' | sudo tee /etc/apt/sources.list.d/owncloud.list


## Updating Raspbian Packages:
You should upgrade the existing packages of your Raspbian OS before you install anything new.

First, update the APT package repository cache with the following command:

$ sudo apt update


*The APT package repository cache should be updated.*

Now, update all the existing packages with the following command:


$ sudo apt upgrade


*Press y and then press <Enter> to continue.*

If you see this message, press q.

*The installation should continue.*

At this point, all the existing Raspbian packages should be upgraded.

Now, reboot your Raspberry Pi with the following command:

$ sudo reboot


## Installing and Configuring Apache and MySQL for OwnCloud:
OwnCloud is a web application that runs on the LAMP (Linux, Apache, MySQL/MariaDB, PHP) stack. So, you need a fully working LAMP server set up before you can install OwnCloud. I am going to show you how to do that in this section.

You can install Apache, PHP, MariaDB and some PHP extensions on Raspbian with the following command:

$ sudo apt install apache2 libapache2-mod-php mariadb-server mariadb-client php-bz2 php-mysql php-curl php-gd php-imagick php-intl php-mbstring php-xml php-zip


*Now, press y and then press <Enter> to continue.*

*All the required packages should be installed.*

Now, run the following command to enable the Apache mod_rewrite module:

$ sudo a2enmod rewrite


*mod_rewrite should be enabled.*

Now, login to the MariaDB console as the root user with the following command:

$ sudo mysql -u root -p


*By default, no MariaDB password is set. So, you can just press <Enter> here without typing in any password. If you had any password set, then you have to type it in here and press <Enter>.*

You should be logged in.

Now, create a new database owncloud with the following query:

MariaDB [(none)]> create database owncloud;


Now, create a new MariaDB user owncloud and also set the password YOUR_PASS for the user with the following query. For simplicity, I am setting the password owncloud for the user owncloud.

MariaDB [(none)]> create user 'owncloud'@'localhost' identified by 'YOUR_PASS';


Now, grant all privileges to the owncloud database to the user owncloud with the following query.

MariaDB [(none)]> grant all privileges on owncloud.* to 'owncloud'@'localhost';


Finally, exit out of the MariaDB shell as follows:

MariaDB [(none)]> exit;


Now, you have to edit the Apache default site configuration file /etc/apache2/sites-enabled/000-default.conf.

To open the Apache default site configuration file /etc/apache2/sites-enabled/000-default.conf, run the following command:

$ sudo nano /etc/apache2/sites-enabled/000-default.conf


Now, find the line as marked in the screenshot below. Then change DocumentRoot /var/www/html to DocumentRoot /var/www/owncloud.

*The final configuration file looks as follows. Now, save the configuration file by pressing <Ctrl> + x followed by y and <Enter>.*

## Installing OwnCloud:
Now, you are ready to install OwnCloud.

To install OwnCloud, run the following command:

$ sudo apt install owncloud-files


*OwnCloud is being installed.*

OwnCloud should be installed at this point.

Now, restart the Apache 2 service with the following command:

$ sudo systemctl restart apache2


## Configuring OwnCloud:
You can find the IP address of your Raspberry Pi with the following command:

$ ip a | egrep "inet "


*As you can see, the IP address of my Raspberry Pi is 192.168.2.6. It will be different for you. So, make sure to replace it with yours from now on.*

Now, from your web browser, visit
