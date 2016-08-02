# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use Ubuntu 14.04 Trusty Tahr 64-bit as our operating system
  config.vm.box = 'debian/jessie64'
  config.vm.hostname = 'Tsuki'
  config.vm.network 'forwarded_port', guest: 3000, host: 3000
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  # config.vm.provision 'shell', path: 'bin/provision.sh', privileged: false
  # Configurate the virtual machine to use 2GB of RAM
  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '1024'
  end

  # Forward the Rails server default port to the host
  config.vm.network :forwarded_port, guest: 3000, host: 3000

  # Use Chef Solo to provision our virtual machine
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", "site-cookbooks"]

    chef.add_recipe "apt"
    chef.add_recipe "nodejs"
    chef.add_recipe "ruby_build"
    chef.add_recipe "ruby_rbenv::system"
    chef.add_recipe "ruby_rbenv::user"
    chef.add_recipe "vim"
    # chef.add_recipe "mysql::server"
    # chef.add_recipe "mysql::client"

  #   # Install Ruby 2.2.1 and Bundler
  #   # Set an empty root password for MySQL to make things simple
    chef.json = {
      rbenv: {
        user_installs: [{
          'user'    => 'vagrant',
          'rubies'  => ['2.3.1'],
          'global'  => '2.3.1',
          'gems'    => {
            '2.3.1'    => [
              { 'name'    => 'bundler'},
              { 'name'    => 'rake' }
            ]
          }
        }]
      },
      # mysql: {
        # server_root_password: ''
      # }
    }
  end
end