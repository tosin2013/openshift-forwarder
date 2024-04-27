OpenShift Forwarder
=========

This Ansible role configures HA Proxy for OpenShift Container Platform (OCP) to provide load balancing and high availability for the OpenShift API and ingress traffic.


Requirements
------------

* OpenShift Container Platform 4.8 or later
* Ansible 2.9 or later

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Prepare Environment
-------------------
```
git clone https://github.com/tosin2013/openshift-forwarder.git /opt/openshift-forwarder
mkdir -p ~/.ansible/roles/
sudo ln -s /opt/openshift-forwarder ~/.ansible/roles/
```


Example Playbook
----------------
Configure the OpenShift Forwarder role in a playbook:
```yaml
mkdir -p playbooks
cat >playbooks/openshift-forwarder.yml<<EOF
- hosts: localhost
  become: true
  roles:
   - openshift-forwarder
EOF
```

Configure the OpenShift Forwarder variables:
```yaml
mkdir -p vars
cat >vars/vars.yml<<EOF
---
# defaults file for openshift-forwarder
haproxy_log_address: "127.0.0.1"
haproxy_chroot_directory: "/var/lib/haproxy"
haproxy_pidfile: "/var/run/haproxy.pid"
haproxy_max_connections: 4000
haproxy_stats_socket: "/var/lib/haproxy/stats"
default_retries: 3
default_timeout_http_request: "10s"
default_timeout_queue: "1m"
default_timeout_connect: "10s"
default_timeout_client: "1m"
default_timeout_server: "1m"
default_timeout_http_keep_alive: "10s"
default_timeout_check: "10s"
default_max_connections: 3000
masters:
  - ip: "192.168.1.10"
  - ip: "192.168.1.11"
  - ip: "192.168.1.12"
workers:
  - ip: "192.168.1.21"
  - ip: "192.168.1.22"
  - ip: "192.168.1.23"
EOF
```
Run the playbook:
```bash
ansible-playbook ./playbooks/openshift-forwarder.yml --extra-vars "@vars/vars.yml" -e "ansible_python_interpreter=/usr/bin/python3" -v
```
on RHEL 8.x, use the following command 
```bash
ansible-playbook ./playbooks/openshift-forwarder.yml --extra-vars "@vars/vars.yml" -e "ansible_python_interpreter=/usr/libexec/platform-python" -v
```


Once Deployment is completed, you can access the HA Proxy stats page using the following URL:
```
http://haproxy-ip-address:1936/haproxy?stats
```
username and password admin:password

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
