
#### 1.  Get the DNS Server IP Address
```ad-pscode
collapse:true
	
	Get-DnsClientServerAddress -InterfaceAlias <interfaceAlias> 
	
```

#### 2. Set the DNS server
```ad-pscode
collapse:true
	# If not configured
	Set-DnsClientServerAddress -InterfaceAlias <InterfaceAlias> -ServerAddresses ("<DNSServerIpAddress>, ...<additionalFallbacks>)
```
