# The Basics

## Setting Up Kali Linux

Firstly, make sure that apt-get is up to date with the with the latest package repositories. During the installation process, you will be asked whether you want to use online repositories or just those included in the installation image. If you forget to set this option, just add the repositories to the sources.list file located at_ /etc/apt/sources.list._

```
deb http://http.kali.org/kali kali-rolling main contrib non-free
# For source package access, uncomment the following line
# deb-src http://http.kali.org/kali kali-rolling main contrib non-free
```

Then, once the repositories have been added, run the following commands to clean the apt-get cache, update the package list from the listed repositories, then update installed packages to their latest versions.

```
apt-get clean
apt-get update
apt-get upgrade
apt-get dist-upgrade
```

## Kali on HDPI Screens

On HDPI screens, such as the Surface Pro 4, fonts, icons and many other aspects of the interface will appear small on the screen  and difficult to use. By default, Kali uses the GNOME to manage the desktop interface. The scaling of the interface can be increased in order to make the interface look and feel better on a HDPI screen.

```
gsettings set org.gnome.desktop.interface scaling-factor 2
```

## Basic Linux Commands

Add a new user:

```
adduser <name> [<group>] 
```



