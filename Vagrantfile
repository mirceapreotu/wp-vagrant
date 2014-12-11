# -*- mode: ruby -*-
# vi: set ft=ruby :


# General project settings
#################################

  vagrant_root_path = File.dirname(__FILE__)

  # IP Address for the host only network(IPv4 private network range)
  ip_address = "172.16.10.10"

  # The project name is base for directories, hostname and alike
  project_name = "wordpress.local"


$script = <<SCRIPT
if [ -f "/var/vagrant_provision" ];
  then exit 0
fi

sudo service mysql restart
mysql -u root -proot -Bse "use wordpress; UPDATE wp_options SET option_value = 'http://#{ project_name }' WHERE option_name IN ('siteurl', 'home');"

sudo touch /var/vagrant_provision
SCRIPT

# Vagrant configuration
#################################

  Vagrant.configure("2") do |config|
    config.vm.box                  = "ubuntu/trusty64"
    config.ssh.private_key_path    = vagrant_root_path + "/.vagrant_keys/vagrant"
    config.hostmanager.enabled     = true
    config.hostmanager.manage_host = true
    config.berkshelf.enabled       = true

    config.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--memory", "1024"]
    end

    config.vm.define project_name do |node|
      node.vm.hostname = project_name
      node.vm.network :private_network, ip: ip_address
    end

    config.vm.provision :hostmanager

    config.vm.provision "shell", inline: "sudo apt-get update"

    config.vm.provision "chef_solo" do |chef|
      chef.add_recipe "mysql::server"
      chef.add_recipe "php::default"
      chef.add_recipe "php-fpm::default"
      chef.add_recipe "nginx::default"
      chef.add_recipe "wordpress::default"

      chef.json = {
          "mysql" => {
              "server_root_password" => "root"
          },
          "php-fpm" => {
              "error_log" => "/var/log/php-fpm.log",
              "log_level" => "debug"
          },
          "wordpress" => {
              "name" => project_name,
              "directory" => "/www/" + project_name,
              "platform" => "nginx",
              "sources" => {
                  "files"    => "https://wordpress.org/latest.zip",
                  "database" => "https://raw.githubusercontent.com/mirceapreotu/stuff/master/111214.wordpress-4.0.1.sql"
              },
              "options" => {
                  "db_name"     => "wordpress",
                  "db_user"     =>  "root",
                  "db_password" =>  "root",
                  "wp_debug"    =>  true
              }
          }
      }
    end

    config.vm.provision "shell", inline: $script
  end