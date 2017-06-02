---
title: Initial Configuration of sNow! Management Node
tags: [domains]
keywords: domains
summary: "The sNow! domain configuration file (domains.conf) provides a table of parameters for each sNow! domain, including the associated roles which define the services provided by each domain (see active-domains.conf) and also the network parameters."
sidebar: mydoc_sidebar
permalink: mydoc_initial_configuration.html
folder: mydoc
---
The sNow! domain configuration file (domains.conf) provides a table of parameters for each sNow! domain, including the associated roles which define the services provided by each domain (see active-domains.conf) and also the network parameters.
This file is generated by executing the ```snow init``` command, but it can be modified to accommodate site specific requirements.

Do it now, and then run:

```
snow init
```
{% include warning.html content="Please, do not run this command in a production environment unless it is required." %}

When you run snow init it not only creates the domains.conf file, it also:
* Creates the NFS configuration (if required) : ${SNOW_CONF}/system_files/etc/exports.d/snow.exports
* Creates domains.conf : ${SNOW_CONF}/system_files/etc/domains.conf
* It updates /etc/hosts with the compute nodes defined in the snow.conf : ${SNOW_CONF}/system_files/etc/static_hosts
* It updates /etc/ssh/ssh_known_hosts
* It sets up /etc/ssh/shosts.equiv
* It sets up the default /etc/ssh/ssh_config

On the domains.conf file the first column contains the hostname of the domain, the second column contains the role or list or roles associated with the domain. The following columns provides information regarding the network configuration such as the network bridge, gateway, netmask, IP, etc.

{% include warning.html content="If you choose ```scenario A``` for your network configuration, be aware that, by default, the secondary network is not setup. If you need to expose a service to the public network, you need to update the ```ip_vif1``` field with an static IP address or with ```dhcp```. The default value is ```none```." %}

Like in active-domains.conf, each domain can have one or more roles. In the case of multiple roles, roles MUST be separated with a colon and without any space in between. See examples below.
If a domain can have multiple roles, each role will be executed in the order defined in this file. More information regarding key sNow! roles is available in active-domains.conf man pages.

* hostname
* role
* vif0
* ip_vif0
* br_vif0
* mac_vif0
* masq_vif0
* gw_vif0
* vif1
* ip_vif1
* br_vif1
* mac_vif1
* masq_vif1
* gw_vif1

The following lines represent a typical example of domains.conf configuration file:

{% include warning.html content="If the sNow! server is also the NFS server, you will need to restart the NFS daemon at this stage. The standard exportfs -ra will not work." %}

```
systemctl restart nfs-kernel-server
```