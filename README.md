# ansible

# Requirements

To install the ansible playbooks:

  git clone https://github.com/purpletortue/ansible  
	cd ansible  
	python3 -m venv env  
	source env/bin/activate  
  pip3 install -r requirements.txt  
  Under playbooks, rename folder vars-example to vars and set vmware_vars.yml to appropriate values  
  Rename inventory-example to inventory and change hostnames to appropriate values  



# TODO

1. dynamic inventories
2. firewalld
3. pw changes
4. Add some error handling
5. docker role
6. apache2 role
7. merge acme with ubuntu-stock
8. add description of each playbook


# Roles
----
ACME
--
Requires 3 'le' variables to be set in vars/main.yml  
- Installs/upgrades acme  
- Configures acme account info  
- Generates server certificate (using aws dns validation)  

COLLECTD
--
Expects fqdn's for inventory host names  
- Installs/updates collectd  
- Configures collectd  
- Copies custom types db  
- Configures collectd to send data to influxdb server named 'collector'  

SERVER
--
Requires 'network' variable to be set in inventory file (see example)  
- Installs/updates minimal set of Python3 modules  
- Installs/updates aws cli  
- Installs/updates cifs-utils  
- Uninstalls/removes snapd  
- Configures rsyslog to forward logs to a host named 'syslog'  

SNMP  
--
Requires 4 variables to be set in vars/main.yml  
- Installs and configures snmpd.  


Copyright and License
---------------------

This software is distributed under The MIT License (MIT)

Copyright (c) 2020 Robert Baptista

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
