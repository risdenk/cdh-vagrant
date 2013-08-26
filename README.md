cdh-vagrant
==============

Vagrant setup for creating Cloudera Manager/CDH test virtual machines

Getting Started
---------------
### Install [Virtualbox](http://www.virtualbox.org/wiki/Downloads)
Download latest version of Virtual and install.

### Install [Vagrant](http://downloads.vagrantup.com)
Download latest version of Vagrant and install.

### Install Vagrant Plugins
Vagrant plugins increase the functionality of Vagrant. They are installed by calling `vagrant plugin install PLUGINNAME`

The plugins to install are: vagrant-hostmanager, vagrant-librarian-chef

### Clone the Repo
1. `git clone https://github.com/risdenk/cdh-vagrant.git`
2. `cd cdh-vagrant`

### Boot the machine
The following steps will download the base VM if it does not exist, boot a single VM, and set the /etc/hosts file up with the correct hostnames.

1. `./up.sh 1` OR `vagrant up u1201`

### Install Cloudera Manager
1. `vagrant ssh u1201`
2. `sudo /tmp/cloudera-manager-installer.bin --i-agree-to-all-licenses --noprompt --noreadme --nooptions`

### Install the Cluster
1. Navigate to `http://u1201.vagrant:7180`
2. Login with username __admin__ and password __admin__
3. For the target host use the FQDN `u1201.vagrant`
4. Upload the __insecure\_private\_key__ from `~/.vagrant.d/insecure_private_key`
5. Specify __vagrant__ as the non-root SSH user
6. Follow the remaining steps
