Install mint cinamon 17.1

Want to set up home directory on separate disk

Install mint with manual partitioning:
http://forums.linuxmint.com/viewtopic.php?f=42&t=122276

For installation type select "Something else"
Then I select sda where my 250gb raptor is inatalled
Select create new partition table
Click "add", we will now create the root partition
I decided to use the whole 300gb - 32gb (swap)
for the root partition this time around.
This will let me figure out what my actual needs are
without worrying about installing lots of software
Swap will be 32gb (assume 16gb ram)
/home is allowed to take all 500gb on sdb


Let the installer finish. Once finished update and upgrade

sudo apt-get update
sudo apt-get upgrade

Install basic stuff
$ sudo apt-get update
$ sudo apt-get install vim curl git python-software-properties
sudo apt-get install build-essential git libgnome-keyring-dev fakeroot

Install chrome
$ cd /tmp
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
$ sudo dpkg -i google-chrome-stable_current_amd64.deb

Add a cool shell
$ sudo apt-get install zsh

$ curl -L http://install.ohmyz.sh | sh

eh didin't switch over - come back to it.


Install desktop theme

Update desktop theme to void
cd ~/Downloads
wget http://cinnamon-spices.linuxmint.com/uploads/themes/ZJF7-L8MD-6U72.zip
unzip the file to the ~/.themes directory
unzip file.zip -d destination_folder

Install virtualbox
on mint 17
sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian trusty contrib" >> /etc/apt/sources.list'
wget http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
sudo apt-get update
sudo apt-get install virtualbox-4.3
sudo apt-get install virtualbox-dkms

*to make usb work in the guest:


*download windows
wget http://msft.digitalrivercontent.net/win/X17-59183.iso

Build atom
*install basics
sudo apt-get install build-essential git libgnome-keyring-dev fakeroot
*install nodejs
curl -sL https://deb.nodesource.com/setup | sudo bash -
sudo apt-get install -y nodejs
git clone https://github.com/atom/atom
cd atom
git fetch -p
git checkout $(git describe --tags `git rev-list --tags --max-count=1`)
script/build
sudo script/grunt install
script/grunt mkdeb
*output is /tmp/atom-build/atom-0.176.0-amd64.deb
sudo dpkg -i /tmp/atom-build/atom-0.176.0-amd64.deb

Setting up Jekyll
sudo apt-get install ruby ruby-dev make gcc nodejs
sudo gem install jekyll --no-rdoc --no-ri

REFS
http://forums.linuxmint.com/viewtopic.php?f=189&t=136770
http://www.erikaheidi.com/blog/setting-up-a-development-machine-with-ubuntu-1404-trusty-tahr
http://www.techsupportalert.com/content/tips-and-tricks-linux-mint-after-installation-mint-17-cinnamon-edition.htm
