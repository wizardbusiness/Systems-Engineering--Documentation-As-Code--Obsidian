##### ***Use To*** Learn about the purpose, logic, and terminology of ansible playbooks

#### Additional References
1.  [Techworld With Nana > What is Ansible - Youtube][1]

[1]: https://www.youtube.com/watch?v=1id6ERvfozo
#### See Also
- [[<Related Doc>]]
### *About Ansible Playbooks*

#### *Summary*
##### Playbooks are Ansibles operational fieldguides for  carrying out IT configuration actions,  which when completed will accomplish a desired configuration outcome on a target system. Playbooks fulfill several important functions - They define what the necessary actions are, how those actions will be completed,  where they will completed, and who they will be completed by. *Tasks* define *what* will be accomplished, *modules* define *how* it will be accomplished, *hosts* define *where* it will lbe accomplished, and the *user* defines *who* it will be accomplished by. . 

#### Written in .YAML

- ##### Widespread adoption
- ##### Human-Readable format
#### Playbook Structure

##### Tasks (the *what*)
- ###### Tasks are defined as one or more modules arranged sequentially that together perform an action. 

```ad-yaml
title: **tasks example**
	tasks: 
	  - name: creatre directory for nginx
		file:
			path: /path/to/nginx/dir
			state: directory
	
	  - name: install nginx latest version 
		yum: 
			name: nginx
			state: latest
		  
	  - name: start nginx
	    service:
			name: nginx
			state: started
```

##### Modules (the *how*)
- ###### Small programs that do the actual work. 
- ###### Each module does one specific task
- ###### Agentless action
	- ###### Get pushed from the control node to the target host and do their work
	- ###### get removed when done.
- ###### Hundreds of modules to accomplish pretty much any IT task you can think of. 

```ad-yaml
title: **module example**
	# excerpt from tasks example
	- name: install nginx latest version 
	    # module
		yum: 
			# module arguments
			name: nginx
			state: latest
```
##### Hosts (the *where*)
- ###### The system(s) on which tasks should be executed
- ###### A host can be any network addressable physical or virtual device running a supported operating system. 
- ###### Also referred to as nodes in the ansible docs
- ###### Can refer to a specific host, or a host *group*

```ad-yaml
title: **Host group example**
	# host group
	- hosts: databases
```
##### User (the *who*)
- ###### Users refer to authenticated users on the device
- ###### Ansible authenticates as the user in order to accomplish tasks
- ###### Ansible requires a user to be an adminstrator with elevated privileges

```ad-yaml
	- hosts: databases
	  # user
	  - remote_user: root
```

```ad-question
**Q: Does the User refer to an actual person who will need to authenticate on Ansibles behalf?**
**A: No, it refers purely to a logical user with permissions within the target host operating system**
```

