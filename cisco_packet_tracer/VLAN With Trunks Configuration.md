
## Initial setup
Prepare the physical layout of the network. In this example network, we will use:

- 6 PCs, 3 in VLAN 10 and 3 in VLAN 20
- 1 additional management PC
- 3 switches

In this tutorial:
- The first VLAN 10 will contain PCs with IP addresses `172.172.172.10-12`. Those PCs will be connected to switch interfaces `FastEthernet 0/1-10`.
- The second VLAN 20 will contain PCs with IP addresses `172.172.172.20-22`. Those PCs will be connected to switch interfaces `FastEthernet 0/11-20`.
- There be VLAN 100 configured for management purposes. The management PC will be connected to switch interface `FastEthernet 0/21`.
- The switches will be connected to each other using trunk ports. The trunk ports will be `GigabitEthernet 0/1` and `GigabitEthernet 0/2`.

Refer to the image below for more info about the network layout, IP addresses, VLANs and interfaces used.

![](../img/vlan_with_trunks_network.png)

## Configure the PCs

- For each of the PC, open its IP configuration and fill in the IP address and mask.

## Configure the switches
When configuring the switches, simply click on the switch, select the CLI tab and terminal will open.

### Switch 1

**Prepare the general stuff:**
- Go to the en environment: `en`
- Enter global configuration mode: `conf t`
- Disable DNS lookups: `no ip domain-lookup`
- Set the hostname to "Switch-1": `hostname Switch-1`
- Enable password encryption for security: `service password-encryption`
- Set enable password: `enable password cisco`

**Create VLAN 10, VLAN 20 and VLAN 100:**
- Create VLAN 10: `vlan 10`
- Set the name of VLAN 10 to "admin": `name admin`
- Create VLAN 20: `vlan 20`
- Set the name of VLAN 20 to "user": `name user`
- Create VLAN 100: `vlan 100`
- Set the name of VLAN 100 to "sprava": `name sprava`
- Exit: `exit`

**Configure the interfaces to be in access mode and assign them to the VLANs:**
- Enter the configuration mode for interfaces FastEthernet 0/1 - FastEthernet 0/10: `interface range f0/1 - 10`
- Set the access mode for the interfaces: `switchport mode access`
- Set the VLAN for the interfaces to VLAN 10: `switchport access vlan 10`
- Switch to the configuration mode for interfaces FastEthernet 0/11 - FastEthernet 0/20: `interface range f0/11 - 20`
- Set the access mode for the interfaces: `switchport mode access`
- Set the VLAN for the interfaces to VLAN 20: `switchport access vlan 20`
- Switch to the configuration mode for interfaces FastEthernet 0/21: `interface f0/21`
- Set the access mode for the interfaces: `switchport mode access`
- Set the VLAN for the interfaces to VLAN 100: `switchport access vlan 100`
- Exit the configuration mode for the interfaces: `exit`

**Next we will prepare for the SSH connection.**
- Configure the vlan 100 interface to have an IP address: `interface vlan 100`, `ip address 192.168.168.10 255.255.255.0`, `no shutdown`, `exit` (Make sure, that the IP address is in the same subnet as the management PC)
- Put the switch into domain: `ip domain-name cisco.ptk`
- Generate an RSA key pair for secure communication with a key size of 1024 bits: `crypto key generate rsa`, than tell it to generate `1024` bit long key
- Create a username "admin" with the password "cisco": `username admin password cisco`
- Change the SSH version to 2: `ip ssh version 2`
- Configure VTY access: `line vty 0 15`, `transport input ssh`, `login local`
- Exit VTY configuration mode: `exit`

**Configure the trunk ports (that will allow the traffic of multiple VLANs to go through multiple switches to the final destination using the same cable):**
- Enter the configuration mode for interfaces GigabitEthernet 0/1 (this interface should be connected to the Switch-2): `interface g0/1`
- Set the trunk mode for the interface: `switchport mode trunk`
- Set the allowed VLANs for the interface: `switchport trunk native vlan 99` - All the VLANs will be allowed to use this trunk.
- Exit the configuration mode for the interface: `exit`

Finish the configuration:
- Exit the configuration mode: `exit`
- Save the configuration: `copy running-config startup-config`

You can also verify the configuration:
- `show vlan` - shows the VLAN configuration
- `show ip interface brief` - shows the interface configuration
- `show interfaces trunk` - shows the trunk configuration


The configuration for Switch 2 and 3 is almost the same. If you are confident enough, feel free to just replicate the configuration from Switch 1. If you are not, follow the steps below (but it is still the same thing, just IP addresses and interfaces might be a bit different).

### Switch 2
**Prepare the general stuff:**
- Go to the en environment: `en`
- Enter global configuration mode: `conf t`
- Disable DNS lookups: `no ip domain-lookup`
- Set the hostname to "Switch-1": `hostname Switch-2`
- Enable password encryption for security: `service password-encryption`
- Set enable password: `enable password cisco`

**Create VLAN 10, VLAN 20 and VLAN 100:**
- Create VLAN 10: `vlan 10`
- Set the name of VLAN 10 to "admin": `name admin`
- Create VLAN 20: `vlan 20`
- Set the name of VLAN 20 to "user": `name user`
- Create VLAN 100: `vlan 100`
- Set the name of VLAN 100 to "sprava": `name sprava`
- Exit: `exit`

**Configure the interfaces to be in access mode and assign them to the VLANs:**
- Enter the configuration mode for interfaces FastEthernet 0/1 - FastEthernet 0/10: `interface range f0/1 - 10`
- Set the access mode for the interfaces: `switchport mode access`
- Set the VLAN for the interfaces to VLAN 10: `switchport access vlan 10`
- Switch to the configuration mode for interfaces FastEthernet 0/11 - FastEthernet 0/20: `interface range f0/11 - 20`
- Set the access mode for the interfaces: `switchport mode access`
- Set the VLAN for the interfaces to VLAN 20: `switchport access vlan 20`
- Exit the configuration mode for the interfaces: `exit`

**Prepare for the SSH connection.**
- Configure the vlan 100 interface to have an IP address: `interface vlan 100`, `ip address 192.168.168.42 255.255.255.0`, `no shutdown`, `exit`
- Put the switch into domain: `ip domain-name cisco.ptk`
- Generate an RSA key pair for secure communication with a key size of 1024 bits: `crypto key generate rsa`, than tell it to generate `1024` bit long key
- Create a username "admin" with the password "cisco": `username admin password cisco`
- Change the SSH version to 2: `ip ssh version 2`
- Configure VTY access: `line vty 0 15`, `transport input ssh`, `login local`
- Exit VTY configuration mode: `exit`

**Configure the trunk ports:**
- Enter the configuration mode for interfaces GigabitEthernet 0/1 (this interface should be connected to the Switch-1): `interface g0/1`
- Set the trunk mode for the interface: `switchport mode trunk`
- Set the allowed VLANs for the interface: `switchport trunk native vlan 99` - All the VLANs will be allowed to use this trunk.
- We will also configure the interface GigabitEthernet 0/2, which will be connected to the Switch-3: `interface g0/2`
- Set the trunk mode for the interface: `switchport mode trunk`
- Set the allowed VLANs for the interface: `switchport trunk native vlan 99`
- Exit the configuration mode for the interface: `exit`

Finish the configuration:
- Exit the configuration mode: `exit`
- Save the configuration: `copy running-config startup-config`


And finally we are going to do the same thing for Switch 3.

### Switch 3
**Prepare the general stuff:**
- Go to the en environment: `en`
- Enter global configuration mode: `conf t`
- Disable DNS lookups: `no ip domain-lookup`
- Set the hostname to "Switch-1": `hostname Switch-3`
- Enable password encryption for security: `service password-encryption`
- Set enable password: `enable password cisco`

**Create VLAN 10, VLAN 20 and VLAN 100:**
- Create VLAN 10: `vlan 10`
- Set the name of VLAN 10 to "admin": `name admin`
- Create VLAN 20: `vlan 20`
- Set the name of VLAN 20 to "user": `name user`
- Create VLAN 100: `vlan 100`
- Set the name of VLAN 100 to "sprava": `name sprava`
- Exit: `exit`

**Configure the interfaces to be in access mode and assign them to the VLANs:**
- Enter the configuration mode for interfaces FastEthernet 0/1 - FastEthernet 0/10: `interface range f0/1 - 10`
- Set the access mode for the interfaces: `switchport mode access`
- Set the VLAN for the interfaces to VLAN 10: `switchport access vlan 10`
- Switch to the configuration mode for interfaces FastEthernet 0/11 - FastEthernet 0/20: `interface range f0/11 - 20`
- Set the access mode for the interfaces: `switchport mode access`
- Set the VLAN for the interfaces to VLAN 20: `switchport access vlan 20`
- Exit the configuration mode for the interfaces: `exit`

**Prepare for the SSH connection.**
- Configure the vlan 100 interface to have an IP address: `interface vlan 100`, `ip address 192.168.168.100 255.255.255.0`, `no shutdown`, `exit`
- Put the switch into domain: `ip domain-name cisco.ptk`
- Generate an RSA key pair for secure communication with a key size of 1024 bits: `crypto key generate rsa`, than tell it to generate `1024` bit long key
- Create a username "admin" with the password "cisco": `username admin password cisco`
- Change the SSH version to 2: `ip ssh version 2`
- Configure VTY access: `line vty 0 15`, `transport input ssh`, `login local`
- Exit VTY configuration mode: `exit`

**Configure the trunk ports:**
- Enter the configuration mode for interfaces GigabitEthernet 0/2 (this interface should be connected to the Switch-2): `interface g0/2`
- Set the trunk mode for the interface: `switchport mode trunk`
- Set the allowed VLANs for the interface: `switchport trunk native vlan 99` - All the VLANs will be allowed to use this trunk.
- Exit the configuration mode for the interface: `exit`

Finish the configuration:
- Exit the configuration mode: `exit`
- Save the configuration: `copy running-config startup-config`



## Testing the network
- Try to ping the PCs in the same VLAN. The ping should be successful.
- Try to ping the PCs in the different VLAN. The ping should be unsuccessful.
- Try to ping the management PC from the switch. The ping should be successful.
- Try to connect to the switch using SSH from the management PC. The connection should be successful.


You can download the final configuration here: [PKT file](./pkt_files/vlanky-trunks.pkt)