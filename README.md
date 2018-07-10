# dmoj-playbook

A set of Ansible playbooks to deploy DMOJ webservers and judges.

### run

You need [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) first(duh).

There are two files to tweak before running:

- `hosts`: State your webserver and judges' IPs or hostnames here. Hostnames are preferred because judges are named based on their `inventory_hostname`.
- `_vars.yml`: This file has only one `var`, which is the IP address the DMOJ bridge daemon should listen on. If the webserver is on the same machine with the judge, this is `127.0.0.1`. Otherwise you would want to configure an internal network containing the webserver and the judges.

Run with `ansible-playbook -i hosts all.yml`.

### contribute

The scripts are relatively simple. We have this TODO though:

- [ ] Add a `common` role/playbook which contains
	- [ ] Passwordless sudo(needed for Ansible)
	- [ ] A firewall task
	- [ ] Separate users for bridged, gunicorn, ...
- [ ] Allow specifying actual judge names instead of hostnames/IP addresses
- [ ] Use the Ansible directory structure everyone uses
