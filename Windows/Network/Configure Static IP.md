1. Get the adapter you want (Usually Ethernet or Wlan)
2. Check current ip address for the adapter
3. If assigned via DCHP remove existing IP Address and netroute
4. Get the default gateway
5. Decide on the ip address (must be unique on network, and in range)
6. Get the cidr prefix length (default: 24)
 ```ad-note
*The prefix length is determined by the network. If you aren't sure what it is find out by checking on a computer that is already up on the network.* 

**Connected Windows Machine:** 

`Get-NetAdapter -InterfaceAlias "<AdapterName(Usually 'Wlan'|'Ethernet')>" | Select-Object PrefixLength`
```

#### Powershell Example Config
```
# show all adapters 
Get-NetAdapter 

# Check if IP was assigned manually or handed out by DHCP
Get-NetIpConfiguration -InterfaceAlias "<AdapterName>" | Select-Object PrefixOrigin, SuffixOrigin

# If prefix and suffix origin are 'Manual' Skip. If DHCP -
Remove-NetIpAddress -InterfaceAlias "<AdapterName>" -Confirm:$false
Remove-NetRoute -InterfaceAlias "<AdapterName>" -Confirm:$false

# Get default gateway
Get-NetIpConfiguration -InterfaceAlias "<AdapterName>" | Select-Object IPv4DefaultGateway

New-NetIPAddress -InterfaceAlias "<AdapterName>" -Confirm:$false -IpAddress <DomainPrefix + UniqueSuffix ex: 172.16.0.112> `
-DefaultGateway <DefaultGateway> -PrefixLength 

```