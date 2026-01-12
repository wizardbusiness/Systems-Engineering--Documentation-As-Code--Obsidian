
# Mokerlink 2G05210GSM CLI Cheatsheet

**Connection:** `telnet 172.16.0.201` (SSH broken - use deprecated algorithms if needed)  
**Default Credentials:** `admin` / `admin`

---

## CLI Modes

```
User EXEC Mode          >        Basic commands, view-only
Privileged EXEC Mode    #        Full command access
Global Config Mode      (config)# System-wide configuration
Interface Config Mode   (config-if)# Per-interface configuration
VLAN Config Mode        (config-vlan)# VLAN configuration
```

### Mode Navigation

```bash
enable                  # User EXEC → Privileged EXEC
configure terminal      # Privileged EXEC → Global Config
exit                    # Go back one level
end                     # Global Config → Privileged EXEC
```

---

## Basic Commands

### Help & Discovery

```bash
?                       # Show available commands
show ?                  # Show all 'show' commands
command ?               # Show command syntax/options
```

### System Information

```bash
show version            # Firmware version, uptime
show running-config     # Current active configuration
show startup-config     # Saved configuration
show system             # System info
```

### Save Configuration

```bash
write memory            # Save running config to startup config
copy running-config startup-config  # Alternative syntax
```

---

## Interface Management

### View Interfaces

```bash
show interface          # All interface status
show interface status   # Port status summary
show interface gigabitethernet 1/0/1  # Specific interface
show interface counters # Traffic statistics
```

### Configure Interface

```bash
configure terminal
interface gigabitethernet 1/0/1
 description "Uplink to Core Switch"
 speed auto             # auto/10/100/1000/2500
 duplex auto            # auto/half/full
 no shutdown            # Enable interface
 shutdown               # Disable interface
exit
```

### Disable/Enable All Ports

```bash
interface range gigabitethernet 1/0/1-5
 shutdown              # Disable ports 1-5
 no shutdown           # Enable ports 1-5
exit
```

---

## VLAN Configuration

### View VLANs

```bash
show vlan              # All VLANs and port assignments
show vlan id 10        # Specific VLAN
show vlan brief        # Summary view
```

### Create VLAN

```bash
configure terminal
vlan 10
 name Management
exit
vlan 20
 name Guest
exit
```

### Delete VLAN

```bash
configure terminal
no vlan 10
exit
```

### Assign Ports to VLAN (Access Mode)

```bash
configure terminal
interface gigabitethernet 1/0/3
 switchport mode access
 switchport access vlan 10
exit
```

### Configure Trunk Port (Multiple VLANs)

```bash
configure terminal
interface gigabitethernet 1/0/1
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30
 switchport trunk native vlan 1
exit
```

### Management VLAN IP

```bash
configure terminal
interface vlan 1
 ip address 192.168.1.10 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.1.1
```

---

## DHCP Configuration

### Enable DHCP Server

```bash
configure terminal
service dhcp           # Enable DHCP service globally
```

### Configure DHCP Pool

```bash
configure terminal
ip dhcp pool VLAN10-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 8.8.4.4
 lease 7                # Days (0 = infinite)
exit
```

### Exclude Addresses from DHCP

```bash
configure terminal
ip dhcp excluded-address 192.168.10.1 192.168.10.10
# Reserves .1-.10 for static IPs (gateway, servers, etc.)
```

### View DHCP Status

```bash
show ip dhcp pool           # All pools
show ip dhcp binding        # Active leases
show ip dhcp server statistics
```

### Disable DHCP Server

```bash
configure terminal
no service dhcp
```

### DHCP Relay (Forward DHCP to Another Server)

```bash
configure terminal
interface vlan 10
 ip helper-address 192.168.1.5  # DHCP server IP
exit
```

---

## Link Aggregation (LACP)

### Create LAG (Port Channel)

```bash
configure terminal
interface range gigabitethernet 1/0/1-2
 channel-group 1 mode active  # active=LACP, on=static
exit
```

### View LAG Status

```bash
show etherchannel summary
show lacp neighbor
```

---

## Spanning Tree Protocol (STP)

### View STP Status

```bash
show spanning-tree          # All VLANs
show spanning-tree vlan 1   # Specific VLAN
```

### Configure STP

```bash
configure terminal
spanning-tree mode rstp     # rapid-pvst/mstp/rstp
spanning-tree vlan 1 priority 4096  # Lower = root bridge
```

### Disable STP on Interface (Edge Port)

```bash
configure terminal
interface gigabitethernet 1/0/5
 spanning-tree portfast     # Skip STP on edge ports
exit
```

---

## Quality of Service (QoS)

### Enable QoS

```bash
configure terminal
mls qos                     # Enable QoS globally
```

### Configure Port Priority

```bash
configure terminal
interface gigabitethernet 1/0/3
 mls qos trust cos          # Trust 802.1p CoS tags
 priority-queue out         # Enable priority queue
exit
```

### View QoS Status

```bash
show mls qos
show mls qos interface gigabitethernet 1/0/3
```

---

## Access Control (ACL)

### Create ACL

```bash
configure terminal
access-list 100 permit ip 192.168.10.0 0.0.0.255 any
access-list 100 deny ip any any
exit
```

### Apply ACL to Interface

```bash
configure terminal
interface gigabitethernet 1/0/5
 ip access-group 100 in
exit
```

### View ACLs

```bash
show access-lists
show ip interface gigabitethernet 1/0/5
```

---

## MAC Address Table

### View MAC Addresses

```bash
show mac address-table           # All learned MACs
show mac address-table interface gigabitethernet 1/0/1
show mac address-table vlan 10
```

### Clear MAC Table

```bash
clear mac address-table dynamic
```

### Static MAC Binding

```bash
configure terminal
mac address-table static 00:11:22:33:44:55 vlan 10 interface gigabitethernet 1/0/3
```

---

## Port Mirroring (SPAN)

### Configure Port Mirror

```bash
configure terminal
monitor session 1 source interface gigabitethernet 1/0/1
monitor session 1 destination interface gigabitethernet 1/0/5
exit
```

### View Mirror Sessions

```bash
show monitor session 1
```

---

## Management & Security

### Change Password

```bash
configure terminal
username admin privilege 15 password NewPassword123
# OR
enable password NewSecurePass
```

### Enable/Disable Telnet

```bash
configure terminal
line vty 0 4
 transport input telnet     # Enable telnet
 no transport input telnet  # Disable telnet
exit
```

### Enable/Disable SSH (if it works)

```bash
configure terminal
ip ssh version 2
crypto key generate rsa modulus 2048
line vty 0 4
 transport input ssh
exit
```

### SNMP Configuration

```bash
configure terminal
snmp-server community public ro   # Read-only
snmp-server community private rw  # Read-write
snmp-server location "Homelab Rack 1"
snmp-server contact "admin@example.com"
```

---

## Troubleshooting

### Ping & Connectivity

```bash
ping 192.168.1.1
ping 192.168.1.1 count 10
traceroute 8.8.8.8
```

### Interface Diagnostics

```bash
show interface gigabitethernet 1/0/1 counters errors
show interface gigabitethernet 1/0/1 status
```

### Clear Counters

```bash
clear counters
clear counters gigabitethernet 1/0/1
```

### Reboot Switch

```bash
reload                     # Reboot (prompts to save)
reload in 5                # Reboot in 5 minutes
```

### Factory Reset

```bash
# Via hardware: Hold reset button 5+ seconds
# Via CLI:
write erase                # Erase startup config
reload                     # Reboot to defaults
```

---

## Common Tasks Quick Reference

|Task|Commands|
|---|---|
|**Check port status**|`show interface status`|
|**Create VLAN 10**|`vlan 10` → `name MyVLAN`|
|**Assign port to VLAN**|`interface gi1/0/3` → `switchport access vlan 10`|
|**Save config**|`write memory`|
|**View running config**|`show running-config`|
|**Enable DHCP pool**|`ip dhcp pool NAME` → `network 192.168.10.0 /24`|
|**Set management IP**|`interface vlan 1` → `ip address 192.168.1.10 /24`|
|**Reboot switch**|`reload`|

---

## Notes

- **Command Completion:** Use `Tab` to autocomplete commands
- **Command Recall:** Use `↑` and `↓` arrow keys for command history
- **Interrupt Output:** Press `Ctrl+C` to stop long output
- **SSH Warning:** SSH on this switch uses deprecated `ssh-rsa` - use Telnet instead
- **Firmware Bugs:** Some firmware versions lose DHCP config on reboot - save frequently
- **Cisco-like, not Cisco:** Some advanced commands may not exist or behave differently

---

**Last Updated:** 2024  
**Switch Model:** Mokerlink 2G05210GSM (5x 2.5GbE + 2x 10G SFP+)  
**Firmware:** Realtek-based, Cisco IOS-style CLI