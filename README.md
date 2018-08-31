# dmoj-playbook

A set of Ansible playbooks to deploy DMOJ webservers and judges.

### run

You need [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) first(duh).

Tweak the `hosts` file before running:
- `webserver_internal_ip` is the IP address the DMOJ bridge daemon should listen on. If the webserver is on the same machine with the judge, this is `127.0.0.1`. Otherwise you would want to configure an internal network containing the webserver and the judges and point `webserver_internal_ip` to the webserver's address on that network.
- `server_name` defines the `server_name` directive for Nginx and the single item in `ALLOWED_HOSTS` for Django.
- Rest of the file: State your webserver and judges' IPs or hostnames here. Hostvars should be specified for judges, in the form of `10.5.6.7 judge_name=dummy`.

Run with `ansible-playbook -i hosts all.yml`.

### run with Vagrant

If you have Vagrant, you can test the playbook on your own computer with VMs: one webserver and `N` judge servers. `N` is 1 by default. You can change N from the `Vagrantfile`. If you set N=0 then there will still be a judge but it will be configured inside the webserver.

Navigate to `dmoj-playbook` directory and run the command `mkdir site`. Run `vagrant up`, wait for the process to finish, then navigate to [127.0.0.1:8080](http://127.0.0.1:8080).

### contribute

To contribute, check out issues. 
