# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

	# BOX_IMAGE = "bento/ubuntu-20.04"
	BOX_IMAGE = "ubuntu/focal64"
	BOX_IMAGE = "hedzr/ubuntu-20-ops-sh"
	BOX_IMAGE = "centos/7"
	BOX_IMAGE = "centos/8"

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  # config.vm.box = BOX_IMAGE

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false


  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL




	# curl https://discovery.etcd.io/new?size=3
	# etcd_cluster = "k8s01.local=http://192.168.56.49:2380"
	etcd_cluster = "k8master.local=http://192.168.56.49:2380"
	# etcd_cluster = "http://k8master.local:2380"
	ip = "192.168.56.31"

	# curl -i https://163.com
	# /vagrant/bin/ops first-install 1 1 192.168.56.31 k8s01.local http://k8s01.local:2380

	# NODE_COUNT = 5

	nodes = Hash.new
	nodes = { # role, tasks are both never used, see ansible vars/main.yml
		"k8mm001" => { "ip"=>"192.168.56.51", "mem"=>3072, "cpu"=>2, "hdd"=>"80GB", "role"=>"M", "tasks"=>["etcd","haproxy","vip","master"] },
		"k8mm002" => { "ip"=>"192.168.56.52", "mem"=>3072, "cpu"=>2, "hdd"=>"80GB", "role"=>"M", "tasks"=>["etcd","haproxy","vip","master"] },
		"k8mm003" => { "ip"=>"192.168.56.53", "mem"=>3072, "cpu"=>2, "hdd"=>"80GB", "role"=>"M", "tasks"=>["etcd","haproxy","vip","master"] },
		# "k8mm004", ..., and more
		"k8ss061" => { "ip"=>"192.168.56.61", "mem"=>3072, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["slave"] },
		"k8ss062" => { "ip"=>"192.168.56.62", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["slave"] },
		"k8ss063" => { "ip"=>"192.168.56.63", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["slave"] },
		"k8ss064" => { "ip"=>"192.168.56.64", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["slave"] },
		"k8ss065" => { "ip"=>"192.168.56.65", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["slave"] },
		# "k8ss004", ..., and more
		
		"redis01" => { "ip"=>"192.168.56.190", "mem"=>1024, "cpu"=>1, "hdd"=>"40GB", "role"=>"M", "tasks"=>["cache", "sentinel"] },
		"redis02" => { "ip"=>"192.168.56.191", "mem"=>1024, "cpu"=>1, "hdd"=>"40GB", "role"=>"S", "tasks"=>["cache", "sentinel"] },
		"redis03" => { "ip"=>"192.168.56.192", "mem"=>1024, "cpu"=>1, "hdd"=>"40GB", "role"=>"S", "tasks"=>["cache", "sentinel"] },
		"redis04" => { "ip"=>"192.168.56.192", "mem"=>1024, "cpu"=>1, "hdd"=>"40GB", "role"=>"S", "tasks"=>["cache", "sentinel"] },
		"redis05" => { "ip"=>"192.168.56.192", "mem"=>1024, "cpu"=>1, "hdd"=>"40GB", "role"=>"S", "tasks"=>["cache", "sentinel"] },
		"redis06" => { "ip"=>"192.168.56.192", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["cache", "master"], "pair"=>"seq01" },
		"redis07" => { "ip"=>"192.168.56.192", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["cache", "slave"], "pair"=>"seq01" },
		"redis08" => { "ip"=>"192.168.56.192", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["cache", "master"], "pair"=>"seq02" },
		"redis09" => { "ip"=>"192.168.56.192", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["cache", "slave"], "pair"=>"seq02" },
		
		"mysql01" => { "ip"=>"192.168.56.180", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"M", "tasks"=>["db", "master"] },
		"mysql02" => { "ip"=>"192.168.56.181", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["db", "slave"] },
		"mysql03" => { "ip"=>"192.168.56.182", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["db", "slave"] },

		"rmq01" => { "ip"=>"192.168.56.170", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"M", "tasks"=>["rmq", "master"] },
		"rmq02" => { "ip"=>"192.168.56.171", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["rmq", "slave"] },
		"rmq03" => { "ip"=>"192.168.56.172", "mem"=>2048, "cpu"=>2, "hdd"=>"80GB", "role"=>"S", "tasks"=>["rmq", "slave"] },

		# "k8cube01" => { "ip"=>"192.168.56.47", "mem"=>4096, "cpu"=>4, "hdd"=>"80GB", "role"=>"CUBE", "tasks"=>["kubecube"] },
	}

	def nat(config, index, hos, atr)
    config.vm.provider "virtualbox" do |v|
			# https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html
			mac = "025eee" + Array.new(6){[*"A".."F", *"0".."9"].sample}.join
      v.customize ["modifyvm", :id, "--nic1", "natnetwork", "--nat-network1", "NatNetwork1010", "--nictype1", "82545EM", "--macaddress1", mac]
			# v.customize ["modifyvm", :id, "--nic2", "hostonly", "--hostonlyadapter2", "VirtualBox Host-Only Ethernet Adapter"]
			# v.customize ["modifyvm", :id, "--nic2", "hostonly"]
			
			# config.ssh.username = 'vagrant'
			# config.ssh.password = 'vagrant'
			# config.ssh.insert_key = 'true'
			
			# config.ssh.private_key_path = "~/.ssh/id_rsa"
			config.ssh.forward_agent = true
			
			# config.vm.network :forwarded_port, id: 'ssh', guest: 22, host: 2222, disabled: true
			# config.ssh.guest_port = 2222
		end
	end

	nodes.each do |hos,atr|
		print hos, ' - ', atr, "\n"

		# config.vm.base_mac = nil   # https://github.com/hashicorp/vagrant/issues/9200

	  config.vm.define hos do |node|
			node.disksize.size = "#{atr['hdd']}"     # vagrant plugin install vagrant-disksize
			# node.vm.disk :disk, size: "120GB", primary: true
			
			node.vm.box = BOX_IMAGE
			node.vm.hostname = "#{hos}.local"

			# node.vm.base_mac = nil
			node.vm.network "private_network", ip: atr['ip'] # , mac: mac

			node.vm.provider "virtualbox" do |v|
				# mac = "025eea45" + Array.new(4){[*"A".."F", *"0".."9"].sample}.join
				# v.customize ["modifyvm", :id, "--macaddress1", mac]
				# v.customize ["modifyvm", :id, "--natnet1", "10.3/16"]
				v.memory = "#{atr['mem']}"
				v.cpus = atr['cpu']
				v.gui = false

				# https://github.com/hashicorp/vagrant/issues/3860#issuecomment-167664778
				### Change network card to PCnet-FAST III
				# For NAT adapter
				# v.customize ["modifyvm", :id, "--nictype1", "Am79C973"]
				# For host-only adapter
				# v.customize ["modifyvm", :id, "--nictype2", "Am79C973"]
				##
				# v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
				# v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
			end
			# node.vm.provision "shell", path: "bin/provision.sh", args: ["first-install", 1, 1, ip, node.vm.hostname, etcd_cluster]
		end
	end

	config.vm.provision "ansible" do |ansible|
		ansible.compatibility_mode = "2.0"
		ansible.playbook = "site.yaml"
		ansible.config_file = "ansible.cfg"
		ansible.groups = {
			"local" => ["localhost"],

			"masters" => ["k8mm00[1:9]"], # masters, control plane
			"slaves" => ["k8ss0[1:9][1:9]"], # workers, data plane
			"kubecube" => ["k8cube[0-9][0-9]"],
			"masters:vars" => {
				"etcd_cluster" => etcd_cluster
			},
			"slaves:vars" => {
				"etcd_cluster" => etcd_cluster
			},
			
			"redis-sentinel" => ["redis0[1:9]"],
			"mysql-cluster" => ["mysql0[1:9]"],
			"rabbitmq-cluster" => ["rmq0[1:9]"],
		}
	end




end
