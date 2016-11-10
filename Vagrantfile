Vagrant.configure(2) do |config|
  config.vm.define "webserver" do |webserver|
    webserver.vm.box = "ubuntu/trusty64"
    #webserver.vm.network "private_network", ip: "192.168.0.2"
    webserver.vm.network "public_network", bridge: "Bridged Adapter"
    webserver.vm.hostname = "webserver"
  end
  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/trusty64"
    #ansible.vm.network "private_network", ip: "192.168.0.254"
    ansible.vm.network "public_network", bridge: "Bridged Adapter"
    ansible.vm.hostname = "ansible"
  end
  
    config.vm.define "testbox" do |testbox|
    testbox.vm.box = "ubuntu/trusty64"
    #testbox.vm.network "private_network", ip: "192.168.0.253"
    testbox.vm.network "public_network", bridge: "Bridged Adapter"
    testbox.vm.hostname = "TestBox"
	
	config.vm.provision "shell", inline: <<-SHELL
	sudo apt-get update
	sudo echo "Welcome to inline script"
	sudo apt-get -y install ansible
	sudo echo [default] >> hosts
	sudo echo 127.0.0.1 >> hosts
	ansible-playbook /vagrant/nginx.yml -i hosts -c local
	SHELL
	
  end
  
end
