# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
	config.vm.define "webserver" do |ws|
		ws.vm.box = "debian/contrib-stretch64"
		ws.vm.provider "virtualbox" do |vb|
			vb.memory = "512"
			vb.customize ["modifyvm", :id, "--cpuexecutioncap", 30]
		end

		ws.vm.hostname = "webserver"
		ws.vm.network "private_network", ip: "192.168.33.10",
			virtualbox__intnet: true
		ws.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

		# Mount site under ./site/. Useful for development.
		# Note that this creates problems if:
		# * package nfs-kernel-server(on Debian) is not installed.
		# * the rpc-statd service is not started.
		ws.vm.network "private_network", ip: "192.168.71.13"
		ws.vm.synced_folder "site/", "/home/vagrant/site/", type: "nfs",
			linux__nfs_options: ["rw", "no_subtree_check", "all_squash", "async"]
	end

	N = 1
	(1..N).each do |n|
		config.vm.define "judge#{n}" do |judge|
			judge.vm.box = "debian/contrib-stretch64"
			judge.vm.provider "virtualbox" do |vb|
				vb.memory = "512"
				vb.customize ["modifyvm", :id, "--cpuexecutioncap", 30/N]
			end

			judge.vm.hostname = "judge#{n}"
			judge.vm.network "private_network", ip: "192.168.33.#{n+10}",
				virtualbox__intnet: true

			# Run Ansible once when all machines are ready.
			# See https://www.vagrantup.com/docs/provisioning/ansible.html
			if n == N
				judge.vm.provision "ansible"  do |ansible|
					ansible.limit = "all"
					ansible.playbook = "all.yml"
					ansible.groups = {
						"webserver" => ["webserver"],
						"judges" => (1..N).map {|n| "judge#{n}"}
					}
					ansible.extra_vars = {
						"webserver_internal_ip" => "192.168.33.10",
						"server_name" => "127.0.0.1"
					}
					ansible.host_vars = {}
					(1..N).each do |n|
						ansible.host_vars["judge#{n}"] = {"judge_name": "judge#{n}"}
					end
				end
			end
		end
	end
end
