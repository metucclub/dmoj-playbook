# dmoj-playbook

A set of Ansible playbooks to deploy DMOJ webservers and judges.

### run

You need [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) first(duh).

There are two files to tweak before running:

- `hosts`: State your webserver and judges' IPs or hostnames here. Hostnames are preferred because judges are named based on their `inventory_hostname`.
- `_vars.yml`: This file has only one `var`, which is the IP address the DMOJ bridge daemon should listen on. If the webserver is on the same machine with the judge, this is `127.0.0.1`. Otherwise you would want to configure an internal network containing the webserver and the judges and

Run with `ansible-playbook -i hosts all.yml`.

### run with Vagrant

If you have Vagrant, ~10G of disk space and  >4GB of RAM you can test the playbook on your own computer with four VMs: one webserver and three judge servers. Run `vagrant up` and see on your own.

### contribute

The scripts are relatively simple. We have this TODO though:

- [ ] Add a `common` role/playbook which contains
	- [ ] Passwordless sudo(needed for Ansible)
	- [ ] A firewall task
	- [ ] Separate users for bridged, gunicorn, ...
- [ ] Allow specifying actual judge names instead of hostnames/IP addresses
- [ ] Use the Ansible directory structure everyone uses
- [ ] Fix async task dependencies:
	- [X] generating assets depend on some Python modules which are not guaranteed to install before they run
	- [ ] Preferably, some other program should fix this whole async thing.
