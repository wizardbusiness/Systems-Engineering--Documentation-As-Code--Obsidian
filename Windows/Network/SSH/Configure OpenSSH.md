
##### **1. Check if Open SSH is installed**
```ad-pscode
collapse:true
	Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
	
	# Check if installed ->
		# OpenSSH.Server~~~~0.0.1.0 
		# OpenSSH.Client~~~~0.0.1.0
```

##### **2. For each uninstalled Open SSH capability add the windows capability** 
```ad-pscode
collapse:true
	Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
	# repeat for Client if needed, but it usually comes preinstalled
```

##### **3. Start the service. Set it to start automatically on boot.** 
```ad-pscode
collapse:true
	Start-Service sshd
	Set-Service -Name sshd -StartupType 'Automatic'
```

##### **4. Enable SSH firewall rules**
```ad-pscode
collapse:true
	Get-NetFirewallRule -Name *ssh* | Where-Object {$_.Enabled -eq 'False'} | Enable-NetFirewallRule
```
	