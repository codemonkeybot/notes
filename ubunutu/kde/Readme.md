# KDE Install on Ubuntu 18

This script is used to deploy KDE, Firefox, dolphin, X2Go Server, Software Properties, Konsole, kgpg, Chrome on Ubuntu 18:

```bash
#!/bin/bash
sudo apt-get -q -y update

export  LANG=en_US.UTF-8

sudo apt-get -q -y --no-install-recommends install gnupg locales && \
    echo "$LANG UTF-8" >> /etc/locale.gen && \
    locale-gen 

sudo apt update && \
    apt upgrade -y && \
    apt install -y plasma-desktop

sudo apt remove -y bluedevil && \
    sed -i 's/.*kdeinit/###&/' /usr/bin/startkde

sudo apt-get -q -y update
sudo apt-get -q -y upgrade

echo "Installing  Firefox"
sudo apt-get -q -y --no-install-recommends install firefox

echo "Installing  Dolphin"
sudo apt-get -q -y --no-install-recommends install dolphin

echo "Installing Software-Properites"
sudo apt-get install -q -y software-properties

echo "Installing X2Go Server"
sudo dpkg --configure -a
sudo apt-add-repository -y ppa:x2go/stable
sudo apt-get -q -y update
sudo apt-get -q -y upgrade
sudo apt-get install -q -y x2goserver x2goserver-xsession

echo "Installing Konsole"
sudo apt-get install -q -y konsole

####################
# Setup Chrome
####################
echo "Installing Google Chrome"
cd /tmp
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add - 
sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt-get -y --allow-downgrades --allow-remove-essential --allow-change-held-packages install -f
rm /tmp/google-chrome-stable_current_amd64.deb
date
echo "Completed Install Google Chrome"
echo "Installation Completed"
exit 0