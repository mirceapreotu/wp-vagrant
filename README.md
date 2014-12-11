# wp-vagrant
WordPress Development environment with Vagrant+Chef

## Prerequisites

* [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Install Vagrant](http://www.vagrantup.com/downloads.htmledit)

Install the following Vagrant plugins
~~~ html
$ vagrant plugin install vagrant-hostmanager
$ vagrant plugin install vagrant-berkshelf --plugin-version 2.0.1
~~~

## Usage
Provision and SSH
~~~ html
$ vagrant up
$ vagrant ssh
~~~

Access your Wordpress installation:
[http://wordpress.local/](http://wordpress.local/)

Administrator credentials:
Username `root`
Password `root`

MySQL administrator password is `root`

## Author
* [Mircea Preotu](https://github.com/mirceapreotu/)

## Release history

version: 1.0.0
- first release