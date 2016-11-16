Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'
  
  {
    'ansible'    => '10.0.0.20',
    'webserver'  => '10.0.0.30',
    'db'         => '10.0.0.40',
  }.each do |short_name, ip|
  
    config.vm.define short_name do |client|
	
	#NETWORK SETTINGS (#VBoxManage.exe list bridgedifs -->to list the possible bridge interface)
	
		#client.vm.network 'private_network', ip: ip
		#client.vm.network "public_network", bridge: 'Broadcom 802.11n Network Adapter', ip: ip
		#client.vm.network "public_network", bridge: 'Realtek PCIe GBE Family Controller', ip: ip
		#client.vm.network "public_network", type: :dhcp
		#client.vm.network "public_network", bridge: "Realtek PCIe GBE Family Controller"  
	#By default, vagrant deletes the default (working) route on additional bridged networks inside the VMs. My problem which was specific for DHCP could only be solved by configuring the bridged network as follows:
	
		#client.vm.network :public_network, :bridge => 'Realtek PCIe GBE Family Controller',:use_dhcp_assigned_default_route => true
	
	# HOSTSUPDATER REQUIRED FIXED IP TO KEEP THE ENTRY INTO HOSTS FILE
		client.vm.network "public_network", bridge: "Realtek PCIe GBE Family Controller", ip: '192.168.7.15'
	

	#VAGRANT-HOSTSUPDATER TO ACCESS VIA FQDN --> NEED ADMIN CMD LINE
	
		#agrant plugin install vagrant-hostsupdater  vagrant-share  --> These are useful plugins 
		client.vm.hostname = "#{short_name}.kishore.com"
	
	# HOST ALIASES
	
		#config.hostsupdater.aliases = ["www.kishore.com", "alias2.kishore.com"]
		
	#SHARING AND PORT FORWARDING
	
		#client.vm.network "forwarded_port", guest: 80, host: 8080
		#client.vm.synced_folder "D:\Download Manager", "/vagrant_data"
		client.vm.synced_folder "../ansible", "/vagrant_data"
   
   # USER NAME AND PASSWORD INSTEAD OF KEY
   
		#client.ssh.username = "vagrant"
		#client.ssh.password = "vagrant"
		#client.ssh.forward_agent = true
		
   # THE BELOW ENTRY WILL AVOID WARNING LIKE TTY NO PRESENT
   		client.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
		
	# SHELL PROVISIONER	   ==> https://www.vagrantup.com/docs/provisioning/shell.html
	    # Script Arguments
	    
			#client.vm.provision "shell" do |s|
			#s.inline = "echo $1"
			#s.args   = "'hello, world!'"
			
	    
	    #EXTERNAL SCRIPT
	    
			#client.vm.provision "shell", path: "script.sh"
			#client.vm.provision "shell", path: "https://example.com/provisioner.sh"
			#inline: "/bin/sh /path/to/the/script/already/on/the/guest.sh"
	    
	   # INLINE SCRIPT
	   
			client.vm.provision "shell", inline: <<-SHELL
					sudo apt-get clean
				SHELL
					
			client.vm.provider "virtualbox" do |v|
				v.name = "#{short_name}.kishore.com"
				v.gui = false
				v.memory = 1024
				v.cpus =2
				v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
				v.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
		
	 end
    end
  end
end
