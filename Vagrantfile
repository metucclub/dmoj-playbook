# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
	# The most common configuration options are documented and commented below.
	# For a complete reference, please see the online documentation at
	# https://docs.vagrantup.com.

	config.vm.provider "virtualbox" do |vb|
		vb.memory = "512"
	end

	config.vm.define "webserver" do |ws|
		ws.vm.box = "debian/contrib-stretch64"
		ws.vm.hostname = "webserver"
		ws.vm.network "private_network", ip: "192.168.33.10"
		ws.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
	end

	N = 3
	(1..N).each do |n|
		config.vm.define "judge#{n}" do |judge|
			judge.vm.box = "debian/contrib-stretch64"
			judge.vm.hostname = "judge#{n}"
			judge.vm.network "private_network", ip: "192.168.33.#{n+10}"	
			# Run Ansible once when all machines are ready.
			# See https://www.vagrantup.com/docs/provisioning/ansible.html
			if n == N
				judge.vm.provision "ansible"  do |ansible|
					ansible.playbook = "all.yml"
					ansible.groups = {
						"webserver" => ["webserver"],
						"judges" => (1..N).map {|n| "judge#{n}"}
					}
				end
			end
		end
	end
end