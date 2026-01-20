#### ***Use To*** build an inventory file for your ansible control node 

#### Prerequisites
- [[Installing Ansible|Ansible]]
- Ansible Project Directory
#### Additional References
- [[1] Building An Inventory - Ansible Docs][1]
 
[1]: https://docs.ansible.com/projects/ansible/latest/getting_started/get_started_inventory.html#get-started-inventory
#### See Also
- [[Create an Ansible Playbook]]
****
### *Info*
##### Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations. Using an inventory file, Ansible can manage a large number of hosts with a single command <sup>[1]</sup>

### Instructions

#### **Create Ansible Inventory**

#### 1. Create an inventory.ini file in your project root directory

```ad-pscode
collapse: true
	New-Item -ItemType File -Name "<project root dir>\inventory.ini"
```
#### **-OR-**

```ad-bash
collapse: true
	touch <project directory>/inventory.ini 
```

#### 2. Add a hosts section to inventory.ini

```ad-pscode
collapse: true
	$hosts = @"
	[your section label aka myhosts]
	<host private ip address> aka 192.168.0.123
	<another host private ip address> aka 192.168.0.124
	<additional host ip addresses...]
	"@
	
	Set-Content -Path <project root dir>\inventory.ini -value $hosts
```
#### **-OR-**

```ad-bash
collapse: true
	nano <project root dir>\inventory.ini
```

```ad-bash
title: **inventory.ini**
collapse: true
	[your section label aka myhosts]
	<host private ip address> aka 192.168.0.123
	<another host private ip address> aka 192.168.0.124
	<additional host ip addresses...]

```

#### 3. Verify your Ansible inventory
```ad-pscode
collapse: true
	ansible-inventory -i inventory.ini --list
```

