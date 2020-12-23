# ansible

# Requirements

To install the ansible playbooks:

  git clone https://github.com/purpletortue/ansible  
	cd ansible  
	python3 -m venv env  
	source env/bin/activate  
  pip3 install -r requirements.txt  
  Rename folder inventory-example to inventory and change/add hostnames to appropriate values  
  Rename folder vars-example to vars and set vmware_vars.yml to appropriate values  
  Rename any vars-example folders within roles to vars and set variables appropriately  

# Roles
* Note: Any playbook in this repo that expects another role to have been run will call that role automatically, but you have to make sure that those role variables are setup and ready to go before hand. For example, the Apache role will automatically call the Acme role so you don't have to, but *you* must make sure the acme variables are set in the acme variables file.
----
ACME
--
Requires 3 'le' variables to be set in vars/main.yml  
- Installs acme  
- Configures acme account info  
- Generates server certificate (using aws dns validation)  

APACHE2
--
Requires completion of acme role  
Requires apache_email variable to be set in vars/main.yml  
- Installs/updates Apache2 & modules  
- Enables Header/SSL modules  
- Disables port 80 and the default site  
- Removes default index.html page  
- Copies ssl cert from acme to apache location  
- Configures virtual host  
- Enables virtual host ssl site  

COLLECTD
--
Expects fqdn's for inventory host names  
- Installs/updates collectd  
- Configures collectd  
- Copies custom types db  
- Configures collectd to send data to influxdb server named 'collector'  

DOCKER
--
- Installs Official Docker repo  
- Installs/updates docker-compose  

NETDATA
--
Requires completion of acme role  
- Installs Official Netdata-edge repo (nightly)  
- Installs/updates Netdata-edge  
- Configures Netdata  
- Configures SSL/TLS for Netdata  

RPI
--
Requires 'network' variable to be set in inventory file (see example)  
Requires freenas variables to be set  
Requires a server user account AND share on truenas to pre-exist  
Requires samba credential file variable to be set  
- Sets timezone
- Installs/updates minimal set of Python3 modules  
- Installs/updates aws cli  
- Installs/updates cifs-utils  
- Configures rsyslog to forward logs to a host named 'syslog'  
- Configures fallback NTP server in timesyncd  
- Installs/udate ssmtp  
- Configures ssmtp to send mail to a host named 'smtp'  
- Configures apt to auto download updates  
- Creates 'nas' user/group  
- Generates random password for truenas server user (if needed)  
- Configures dir/share mount at /mnt/nas  
- Restores a backup cron file from /mnt/nas/backup/cron if it exists  
- Configures a backup cron file at /etc/cron.d/backup  

SERVER
--
Requires 'network' variable to be set in inventory file (see example)  
Requires freenas variables to be set  
Requires a server user account & share on freenas to pre-exist  
Requires samba credential file variable to be set  
- Sets timezone
- Installs/updates minimal set of Python3 modules  
- Installs/updates aws cli  
- Installs/updates cifs-utils  
- Uninstalls/removes snapd  
- Configures rsyslog to forward logs to a host named 'syslog'  
- Configures fallback NTP server in timesyncd  
- Installs/udate ssmtp  
- Configures ssmtp to send mail to a host named 'smtp'  
- Configures apt to auto download updates  
- Creates 'nas' user/group  
- Generates random password for freenas server user (if needed)  
- Configures dir/share mount at /mnt/nas  
- Restores a backup cron file from /mnt/nas/cron if it exists  
- Configures a backup cron file at /etc/cron.d/backup  

SNMP  
--
Requires 4 variables to be set in vars/main.yml  
- Installs and configures snmpd.  

TELEGRAF
--
Expects fqdn's for inventory host names  
Requires fqdn variable for influxdb_server in vars/main.yml  
Requires completion of acme role  
Requires FreeNAS variables for API access  
For FreeNAS hosts requires telegraf binary to be downloaded and path set in variables file  
Requires 'telegraf' user to pre-exist on FreeNAS/TrueNAS hosts
- Adds official Influxdata repo (except on Free/TrueNAS)  
- Creates & copies files to /opt/telegraf on Free/TrueNAS  
- Installs/updates telegraf  
- Configures telegraf  
- Configures telegraf to send data to 'influxdb_server'  

# Applications
* Note: All application deployments/configurations below are expecting the SERVER role to have been applied/configured already.
----

Unifi Controller
--
Expects 'unifi_controller' group in inventory file with an fqdn host entry  
Requires completion of acme role  
- Installs prerequisite packages  
- Installs Official Unifi repo from Ubiquiti  
- Installs/updates Unifi and MongoDB from unifi repo  
- Adds unifi user to remote storage group (for backups)  
- Configures Java with custom SSL/TLS cert  
- Configures MongoDB and Unifi Controller  

# TODO

* dynamic vm inventory (once vmware inv module works with standalone)  
* firewalld  
* pw changes (user & root)  
* cron backup management  
* convert ups shutdown automation  
* add freenas user creation  
* add freenas share configuration  
* add freenas dataset creation  
* Figure out how to upgrade acme only when necessary, previous method always showed as *changed*  
* Migrate most playbook/role variables to 'global' files (ie. group_vars)  
* add FreeNAS telegraf PostInit task via API when available(see freenas.yml file)  

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
