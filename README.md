# dmoj-playbook

A set of Ansible playbooks to deploy DMOJ webservers and judges.

### run

You need [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) first(duh).

There are two files to tweak before running:

- `hosts`: State your webserver and judges' IPs or hostnames here. Hostvars should be specified for judges, in the form of `10.5.6.7 judge_name=dummy`.
- `_vars.yml`: This file has two `var`s.
	- `webserver_internal_ip` is the IP address the DMOJ bridge daemon should listen on. If the webserver is on the same machine with the judge, this is `127.0.0.1`. Otherwise you would want to configure an internal network containing the webserver and the judges and point `webserver_internal_ip` to the webserver's address on that network.
	- `server_name` defines the `server_name` directive for Nginx and the single item in `ALLOWED_HOSTS` for Django.

Run with `ansible-playbook -i hosts all.yml`.

### run with Vagrant

If you have Vagrant, ~10G of disk space and  >4GB of RAM you can test the playbook on your own computer with VMs: one webserver and `N` judge servers. `N` is 3 by default, but it is advised to reduce it on a computer(see the `Vagrantfile`) which is not a i7 16GB development laptop.

Run `vagrant up`, wait for 30-60 minutes, then navigate to [127.0.0.1:8080](http://127.0.0.1:8080).

### contribute

The scripts are relatively simple. We have this TODO though:

- [ ] Create separate users for bridged, gunicorn, ...
- [X] Allow specifying actual judge names instead of hostnames/IP addresses
- [ ] Use the Ansible directory structure everyone uses
- [ ] Fix async task dependencies:
	- [X] generating assets depend on some Python modules which are not guaranteed to install before they run
	- [ ] Preferably, some other program should fix this whole async thing.
- [ ] Ensure idempotency on tasks
