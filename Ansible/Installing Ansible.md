****
##### ***Use this doc when** You want to create an [[_Ansible Glossary#Ansible|ansible]] [[_Ansible Glossary#Control Node|control node]]

#### Additional Resources:
- [Ansible Docs - Getting Started](https://docs.ansible.com/projects/ansible/latest/getting_started/introduction.html)
- [Installing Ansible - Ansible Docs](https://docs.ansible.com/projects/ansible/latest/installation_guide/intro_installation.html#installation-guide)

#### Prerequisites
- [[Install WSL Manually - Windows Server Core|WSL (Windows Subsystem For Linux)]] 
- [[Python]]
- [[Pip]]

***
### *Preface*
##### There are a lot of ways to install [[_Ansible Glossary#Ansible|Ansible]], check out the [Ansible Docs - Getting Started](https://docs.ansible.com/projects/ansible/latest/getting_started/introduction.html)  web page for more. The easiest way IMO is to just use pip to download the Ansible package so thats what this doc focuses on.

### Instructions

#### **Install via pip**
#### 1. Verify that pip is installed 
*If not installed, see *
```ad-ad-py
collapse: true
	python3 -m pip -V
```

#### 2. *pip install* ansible with python version of your choice 
*Note that ansible is installed for the current user only*
```ad-ad-py
collapse: true
	python3 -m pip install --user ansible
```

