# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

boxes = [
         {:name => "u1201", :ipaddress => "192.168.12.101", :roles => ["base", "cloudera-manager"], :recipes => [], :json => {}},
         {:name => "u1202", :ipaddress => "192.168.12.102", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1203", :ipaddress => "192.168.12.103", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1204", :ipaddress => "192.168.12.104", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1205", :ipaddress => "192.168.12.105", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1206", :ipaddress => "192.168.12.106", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1207", :ipaddress => "192.168.12.107", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1208", :ipaddress => "192.168.12.108", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1209", :ipaddress => "192.168.12.109", :roles => ["base"], :recipes => [], :json => {}},
         {:name => "u1210", :ipaddress => "192.168.12.110", :roles => ["base"], :recipes => [], :json => {}},
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  
  boxes.each do |opts|
    config.vm.define opts[:name] do |vmconfig|
      vmconfig.vm.box = "ubuntu-precise-64"
      vmconfig.vm.box_url = "http://files.vagrantup.com/precise64.box"

      vmconfig.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", 512] # RAM allocated to each VM
      end

      vmconfig.vm.provider :aws do |aws, override|
        aws.keypair_name = "lockeypair"
        override.ssh.private_key_path = "~/.ssh/pk-lockeypair.pem"
        aws.instance_type = "m1.small"
        aws.security_groups = "Ambari"
        aws.ami = "ami-7d0c6314"

        override.ssh.username = "ubuntu"
        aws.tags = {
          'Name' => 'cloudera-manager',
        }

        override.omnibus.chef_version = :latest
      end
      
      vmconfig.vm.hostname = "%s.vagrant" % opts[:name].to_s
      vmconfig.vm.network :private_network, ip: opts[:ipaddress]

      vmconfig.vm.provision :chef_solo do |chef|
        chef.cookbooks_path = ["cookbooks", "site-cookbooks"]
        chef.data_bags_path = "databags"
        chef.roles_path = "roles"

        opts[:roles].each do |role|
          chef.add_role(role)
        end

        opts[:recipes].each do |recipe|
          chef.add_recipe(recipe)
        end
      end
    end
  end
end
