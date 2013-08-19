$project_name = 'vagrant-typo3'

# ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- -----

Vagrant.require_plugin('vagrant-hostmanager')
Vagrant.configure("2") do |config|

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.hostname = "www.#{$project_name}.dev"
  #config.vm.network :private_network, ip: "192.168.144.101"
  config.vm.network :public_network, :bridge => 'en1: Wi-Fi (AirPort)'

  # automatically manage /etc/hosts on hosts and guests
  config.hostmanager.enabled = false
  config.hostmanager.manage_host = true

  config.vm.synced_folder 'htdocs', '/var/www/site-' + $project_name,
    :owner=>'www-data', :group=>'www-data', :extra => 'dmode=775,fmode=775'

  # forward http
  config.vm.network :forwarded_port, guest: 80, host: 8800
  # forward MySQL
  config.vm.network :forwarded_port, guest: 3306, host: 33060

  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |vb|
    vb.name = "www.#{$project_name}.dev"

    vb.gui = false

    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
  end

  config.vm.provision :chef_solo do |chef|

    # Chef run list
    chef.add_recipe 'apt'
    chef.add_recipe 'vim'
    chef.add_recipe 'networking_basic'
    chef.add_recipe 'mysql::server'
    chef.add_recipe 'php'
    chef.add_recipe 'php::module_gd'
    chef.add_recipe 'php::module_apc'
    chef.add_recipe 'apache2'
    chef.add_recipe 'graphicsmagick'
    chef.add_recipe 'typo3'
    chef.add_recipe 'typo3::scheduler'
    chef.add_recipe 'training'

    chef.json = {
      :mysql  => {
        :server_root_password   => "password",
        :server_repl_password   => "password",
        :server_debian_password => "password",
        :allow_remote_root      => true,
        :bind_address           => "127.0.0.1"
      },
      :git    => {
        :prefix => "/usr/local"
      },
      :apache => {
        :default_site_enabled => false
      },
      :typo3  => {
        :site_name => $project_name,
        :package => 'bootstrap'
      }
    }
  end

  # run host manager after chef
  config.vm.provision :hostmanager

end
