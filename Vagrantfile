# coding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

$shell = <<SCRIPT
### optional #############################
sudo -u postgres createuser -d -S -R test_db_user
sudo -u postgres createdb -U test_db_user -E utf-8 test_db

sudo mysql --user=root --password=password -e "CREATE DATABASE test_db;"
sudo mysql --user=root --password=password -e "GRANT ALL ON test_db.* TO test_db_user@'%' IDENTIFIED BY 'password';"

## 以下のコメントを有効にすると, remote_db のコピーをローカルに作成する
#
# sudo -u vagrant echo "remote_host:5432:remote_db:remote_db_user:password" > .pgpass
# chown vagrant:vagrant .pgpass
# chmod 600 .pgpass
# sudo -u vagrant pg_dump -h remote_host -U remote_db_user remote_db | psql -U test_db_user test_db
### END optional #########################

echo 'Congratulations!!! Install Success. Please access http://localhost:8888'
SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "nrel/CentOS-6.5-i386"
  config.vm.hostname = "centos"

  config.vm.network :forwarded_port, guest: 80, host: 8888
  config.vm.network :forwarded_port, guest: 443, host: 8443
  config.vm.synced_folder ".", "/var/tmp/www", :mount_options => ["dmode=777,fmode=666"]

  config.vm.provision :chef_solo do |chef|
    # chef.log_level = :debug
    chef.cookbooks_path = ["chef/cookbooks", "chef/site-cookbooks"]

    chef.roles_path = "chef/roles"
    chef.data_bags_path = "chef/data_bags"

    chef.add_recipe     "iptables::disabled"
    chef.add_recipe     "apache2"
    chef.add_recipe     "apache2::mod_ssl"
    chef.add_recipe     "apache2::mod_rewrite"
    chef.add_recipe     "postgresql::client"
    chef.add_recipe     "postgresql::server"
    chef.add_recipe     "php::source"
    chef.add_recipe     "mysql::client"
    chef.add_recipe     "mysql::server"

    chef.json = {
      :apache => {
        :version => "2.2",
        :default_site_enabled => true,
        :docroot_dir => "/var/tmp/www/public_html",
        :listen_ports => [80, 443]
      },
      :php => {
        :install_method => "source",
        :url => "http://museum.php.net/php5",
        :version => "5.3.9",
        :checksum => "424c6312ab23d1f8b0135cd52c2c08054d1ee9b21691e5ca3ebb0775670b7aee",
        :directives => {
          :display_errors => 'On',
          "date.timezone" => "Asia/Tokyo",
        },
      },
      :postgresql => {
        :password => {
          postgres: 'password'
        },
        :pg_hba => [
          {:type => 'local', :db => 'all', :user => 'postgres', :addr => nil, :method => 'trust'},
          {:type => 'local', :db => 'all', :user => 'all', :addr => nil, :method => 'trust'},
          {:type => 'host', :db => 'all', :user => 'all', :addr => '127.0.0.1/32', :method => 'trust'},
          {:type => 'host', :db => 'all', :user => 'all', :addr => '::1/128', :method => 'trust'}
        ]
      },
      :mysql => {
        :version => "5.5",
        :server_root_password => "password"
      }
    }
  end

  config.omnibus.chef_version = '12.2.1'
  # config.berkshelf.enabled = true

  config.vm.provision "shell", inline: $shell
end
