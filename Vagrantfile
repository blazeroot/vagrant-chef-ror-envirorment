# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box     = "precise-server-clouding-amd64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', 4096]
    vb.customize ['modifyvm', :id, '--cpus',   4   ]
   end

  config.omnibus.chef_version = :latest

  config.vm.network :forwarded_port, guest: 3000, host:3030

  config.berkshelf.enabled = true

  config.vm.provision :chef_solo do |chef|
    chef.node_name       = 'looplay-develop'
    
    chef.roles_path      = 'roles'
    chef.data_bags_path  = 'data_bags' 

    chef.add_recipe 'apt'
    chef.add_recipe 'git'
    chef.add_recipe 'build-essential'
    chef.add_recipe 'zlib'
    chef.add_recipe 'rbenv::user'
    chef.add_recipe 'ruby_build'
    chef.add_recipe 'postgresql'
    chef.add_recipe 'zsh'
    chef.add_recipe 'tmux'
    chef.add_recipe 'dotfiles'

    chef.json = {
      'rbenv' => {
        'user_installs' => [
          {
            'user'   => 'vagrant',
            'rubies' => ['1.9.3-p448', '2.0.0-p247'],
            'global' => '1.9.3-p448',
            'gems'   => {
              '1.9.3-p448' => [
                { 'name' => 'bundler' }
              ]
            }
          }
        ]
      },
      "postgresql" => {
        "password" => {
          "postgres" => "postgres"
        }
      },
      'dotfiles' => {
        'user' => {
          'name' => 'vagrant'
        }
      }
    }

    chef.add_role('appserver')
 
    chef.run_list = [
      'recipe[git]',
      'recipe[ruby_build]',
      'recipe[rbenv::user]',
      'recipe[zsh]',
      'recipe[postgresql]',
      'recipe[tmux]',
      'recipe[dotfiles]'
    ]

   end

end
