
##### ***Use To***  Add a host to be managed by Ansible via the Ansible Control Node Inventory

#### Prerequisites
- [[Create an Ansible Inventory]]
#### Additional References
-  [[1] example.com reference alias][1]

[1]: www.example.com
#### See Also
- [[<Related Doc>]]
### *Info*
##### Ansible control nodes can use either SSH or WInRM protocols to connect to a host. There are also various methods to handle host authentication and credential storage that are more or less suited to different kinds of environments. This guide covers methods for configuring your Ansible inventory for both Windows and Linux hosts,  as well as for lab and production environments.   

***
### Adding a Windows Host to the Inventory - Basic Auth Method

```ad-warning
**Do not use in production environments. Only do this when getting familiar with Ansible or prototyping. Only use this method in test and lab environments where security is not a concern. Use Kerberos or Ansible Vaults in production**
```

#### 1. Navigate to the Ansible root folder containing the control node Inventory.ini
```ad-bash
collapse: true
	cd /path/to/ansible/inventory
```

#### 2. Open up the inventory.ini in a text editor like nano
```ad-bash
collapse: true
	 nano inventory.ini
```

#### 3. Add windows host config (username, password, winrm enabled)
```ad-bash
collapse: true
	# Add [section name] if no other windows hosts exist
	[windows] 
	win_host_1 ansible_host=1982.168.1.123
	
	[windows:vars]
	ansible_user=host_admin_username
	ansible_password=host_admin_password
	ansible_port=5985
	ansible_connection=winrm
	ansible_winrm_server_cert_validation=ignore
	# next host
	win_host_2
	...
```

#### 4. Test connection to host
```ad-bash
collapse: true
	# 'windows' is the section name, substitute for whatever your section name is
	ansible windows win_ping -i inventory.ini
```

###### ***Troubleshooting***
![[Host Unreachable - Failed to establish a new connection]]