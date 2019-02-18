  # Commands
  
  * Add a login password
  ```
  Switch(config)#line console 0
  Switch(config-line)#password <pass>
  Switch(config-line)#login
  ```
  
  * Add an MOTD
  ```
  Sofiane(config)#banner motd "<message>"
  ```
  
  * Save the running config (**WORKS IN SHOW MODE ONLY**)
  Saves the RAM config to the NVRAM
  ```
  Switch#copy Running-config Startup-config
  ```
  
  * Show IP addresses of all interfaces
  ```
  Switch#show ip interface brief
  ```
  
  * Show arp entries of a router
  ```
  Router#show ip arp
  ```
  
  * Show used routing protocols
  ```
  Router#show ip protocol
  ```
  
  * Range interfaces
  ```
  Switch(config)#interface range fastethernet 0/2-3
  ```
  
  * Activate RIP routing
    * Configure NICs addresses
    * Activate RIP router
    * Manually add directly connected networks
  
  
  **RIP**
  ``` 
  #... NICs config
  Router(config)#router rip
  Router(config-router)#network 10.0.0.0
  Router(config-router)#network 11.0.0.0
  ```
  **RIPv2**
  ```
  #... NICs config
  Router(config)#router rip
  Router(config-router)#version 2 #IN CASE OF RIPv2 ONLY
  Router(config-router)#network 10.0.0.0
  Router(config-router)#network 11.0.0.0
  Router(config-router)#no auto-summary
  ```
  
  * Activate OSPF routing
    * Configure NICs addresses
    * Activate OSPF routing
    * Manually add directly connected networks with wildcard-Mask
  
  ```
  #... NICs config
  Router(config)#router ospf 100 
  Router(config-router)#network 10.0.0.0 0.255.255.255 area 0
  Router(config-router)#network 11.0.0.0 0.255.255.255 area 0
  ```
  
  * Clear OSPF routing table
  ```
  Router#clear ip ospf process
  ```
  
  * Add a default route in a router
  ```
  (conf)# ip route 0.0.0.0 0.0.0.0 20.0.0.2
  ```
  
  
  * Create a VLAN
  ```
  Switch(config)#vlan 2
  Switch(config-vlan)#name IT
  ```
  
  
  * Configure an interface in access mode
  ```
  Switch(config-if)#switchport mode access
  ```
  
  * Configure an interface in trunk mode
  ```
  Switch(config-if)#switchport mode trunk
  ```
  
  * Affect a vlan to an interface
  ```
  Switch(config-if)#switchport access vlan 2
  ```
  
  * Create a subinterface in a router
  ```
  Router(config)#interface gigabitethernet 0/0.<number>
  ```
  
  * Add vlans to router
  ```
  Router(config)#interface gigabitethernet 0/<physical_int_number>.<sub_int_number>
  Router(config-subif)#encapsulation dot1q <vlan> [native]
  Router(config-subif)#ip address <ip_addr> <mask>
  ```
  
  * Add DHCP in router
  ```
  Router(config)#ip dhcp pool name
  Router(dhcp-config)#network 20.0.0.0 255.0.0.0
  Router(dhcp-config)#default-router 20.0.0.1 
  Router(dhcp-config)#dns-server 8.8.8.8
  # TFTP server
  Router(dhcp-config)#option 150 ip <ip_address>
  ```
  
  * Check DHCP informations
  ```
  Router#show ip dhcp pool
  
  Router#show ip dhcp binding
  ```
  
  * Remove config (except VLANS)
  ```
  Switch#write erase
  Switch#reload
  ```
  
  * Show flash content
  ```
  Switch#dir flash
  ```
  
  * Remove content from flash
  ```
  Switch#delete flash:vlan.dat
  ```
  
  * Show trunk interfaces
  ```
  Switch# show interface trunk
  ```
  
  * Activate port security in a switch
  ```
  Switch(config-if)#switchport mode access 
  Switch(config-if)#switchport port-security 
  Switch(config-if)#switchport port-security maximum 1 
  ```
  
  * Choose what happens after a port security violation
  ```
  Switch(config-if)#switchport port-security violation (shutdown|protect|restrict)
  ```
  
  * Port security with mac address
    Either enter mac address 
      * manually 
      * "sticky" to grab the first connected Mac address
    The saved MAC addresses are in the running-cofig, to keep them permenant copy the config to the startup-config
  ```
  Switch(config-if)#switchport port-security mac-address <mac_address|sticky> 
  ```
  
  
  * Add an enable password 
  ```
  # Unencrypted
  Switch(config)#enable password sof
  # Encrypted
  Switch(config)#enable secret sof
  # Encrypted using different algorithm
  Switch(config)#enable algorithm-type <algo-type> secret sof
  ```
  
  * Remove enable password
  ```
  no enable password
  ```
  
  * **Encrypt any password (old or new)**
  ```
  Switch(config)#service password-encryption
  ```
  
  * Change exec-timeout (auto logout )
  ```
  Router(config-line)#exec-timeout <min> [sec]
  ```
  If min = sec = 0 it means no auto logout
  
  * Associate an address to a switch
  ```
  Switch(config)#interface vlan 1
  Switch(config-if)#ip address 10.0.0.100 255.0.0.0
  Switch(config-if)#no shutdown
  ```
  
  * Add a def gateway to the switch
  ```
  Switch(config)#ip default-gateway 192.168.1.1
  ```
  
  * Activate Telnet
  ```
  #... give the switch an IP address
  # 0 -> 4: 5 users can connect at a time; can reach up to 15 at a time
  Switch(config)#line vty 0 4
  Switch(config-line)#password sofiane
  Switch(config-line)#login
  Switch(config-line)#exit
  # Add an encrypted password
  Switch(config)#enable secret sof
  ```
  
  * Create an standard numbered ACL policy 
  ```
  #access-list <id> <deny|permit> <network_ip> <wildcard_mask>
  (Conf)# access-list 50 deny 11.0.0.0 0.255.255.255
  ```
  
  * Attribute a numbered ACL to an interface
  ```
  #ip access-group <ACL_ID> <in|out>
  (Conf-if)# ip access-group 50 in
  ```
  
  * Create an extended numbered ACL policy
  Either enter IP of a single device only or use network with wildcard mask
  ```
  (Conf)# access-list <id> <deny|permit> (TCP|UDP) [host] <source_IP> [wildcard_mask] [host] <destination_IP> [wildcard_mask] [eq <port>]
  ```
  
  * Create Named ACL
  ```
  Router(config)#ip access-list <extended|standard> <name>
  Router(config)#[acl_order_number] <deny|permit> <proto> host 10.0.0.3 host 11.0.0.3
  Router(config-ext-nacl)#deny icmp host 10.0.0.3 host 11.0.0.3
  Router(config-ext-nacl)#permit icmp any any 
  ```
  
  * Configure static NAT
  
  ```
  Router(config)#int g0/0
  Router(config-if)#ip nat inside 
  Router(config-if)#int g0/1
  Router(config-if)#ip nat outside 
  Router(config-if)#ip nat inside source static <inside_local_address> <inside_global_address>
  ```
  
  
  * Configure dynamic NAT
  An access list must be added 
  ```
  (conf-if0)# ip nat inside
  (conf-if1)# ip nat outside
  (conf)# ip nat pool <nat_name> <start_ip> <end_ip> netmask <mask> 
  (conf)# access-list <standard_id> permit <inside_network> <wildcard_mask>
  (conf)# ip nat inside source list <acl_id> pool <nat_name> [overload] #overload is added to use only 1 external address
  ```
  
  * Check NAT translations
  
  ```
  Router#show ip nat translations
  ```
  
  * Activate routing distribution
  ```
  Router(config)#router ospf 1
  Router(config-router)#redistribute static subnets
  ```
  * Activating L3 on a MLS
  ```
  Switch(config)#ip routing
  ```
  
  * Configure inter-VLAN on a MLS
  ```
  
  Switch(config)#ip routing
  Switch(config)#vlan 2
  Switch(config)#vlan 3
  Switch(config)#int range fa0/1-2
  Switch(config-if)#switchport mode access
  Switch(config-if)#switchport access vlan 2
  ***repeat for vlan 3***
  Switch(config)#int vlan 2
  Switch(config-if)#ip address 20.0.0.1 255.0.0.0
  Switch(config-if)#no shutdown
  ***repeat for vlan 3***
  
  ```
  
  * Check the version of a device
  ```
  #show version
  ```
  
  * Password recovery
    * Phyisically reboot the device
    * Interupt the boot with Ctrl-C
  ```
  confreg 0x2142
  reset
  ```
  
  * Backup/Restore config
  ```
  **backup**
  Router# copy startup-config tftp:
  
  **restore **
  Router# copy tftp: running-config
  
  ```
  
  * VTP configurations 
  ```
  # Change vtp mode 
  Switch(config)#vtp mode <client|server|transparent>
  # Change vtp domain
  Switch(config)#vtp domain <domain>
  # Change vtp password
  Switch(config)#vtp password <password>
  # Activate VTP prunning 
  Switch(config)#vtp pruning
  # Set the VTP version
  Switch(config)#vtp version
  ```
  
  * Show VTP status
  ```
  show vtp status
  ```
  
  * Show DTP switchport mode of an interface
  ```
  Switch#show interfaces f0/1 switchport 
  ```
  
  * Change Switch DTP switchport mode
  ```
  Switch(config-if)#switchport mode <access|trunk|dynamic <auto|desirable> 
  ```
  
  * Allowed DTP
  ```
  Switch(config-if)#switchport trunk allowed vlan 
    ** add     add VLANs to the current list
    ** all     all VLANs
    ** except  all VLANs except the following
    ** none    no VLANs
    ** remove  remove VLANs from the current list
  ```
  
  * CDP 
  ```
  Switch#show cdp neighbors [detail]
  Switch#show cdp entry <device_id>
  Switch#show cdp interface <interface>
  ```
  
  * Stop CDP
  ```
  #Globaly
  Switch(config)#no cdp run
  #Interface
  Switch(config-if)#no cdp enable
  ```
  
  * Change the STP mode 
  ```
  (conf)#spanning-tree mode (pvst|rapid-pvst)
  ```
  
  * Change the STP priority of a switch
  ```
  (conf)#spanning-tree vlan <vlan_num> priority <num>
  # or 
  (conf)#spanning-tree vlan 1 root (primary|secondary) 
  ```
  
  * Change the STP wait time (Listening, Learning) on end devices ports (mode access)
  ```
  (config)#spanning-tree portfast default 
  # or 
  (conf-if)#spanning-
  ```
  
  * Deny the reception of BPDUs on an interface (BPDU guard)
  ```
  (config-if)#spanning-tree bpduguard enable
  ```
  
  * Root guard
  ```
  (config-range-if)# spanning-tree guard root
  ```
  
  * Change the port priority of an interface
  ```
  (config-if)#spanning-tree vlan 1 port-priority <0-240<
  ```
  
  * Show the time of a device
  ```
  #show clock
  ```
  
  * Set the time of a device
  ```
  #clock set 
  ```
  
  * Set an NTP server
  ```
  (conf)#ntp server <addr>
  ```
  
  * Configure a loopback
  Loopback is a software interface
  It's always up
  from 0 to 2,1bil
  
  ```
  Router(config)#int loopback 0
  Router(config-if)#ip address 10.0.0.1 255.0.0.0
  ```
  
  * EIGRP routing
  ```
  Router(config)#router eigrp 100 
  Router(config-router)#network 10.0.0.0 0.255.255.255
  Router(config-router)#network 11.0.0.0 0.255.255.255
  
  Router(config-router)#no auto-summary   # Add this in case of using subnetting but not summarization
  ```
  
  * PPP config
  ```
  Router(config-if)#encapsulation ppp
  Router(config-if)#ppp authentication chap 
  Router(config-if)#exit
  Router(config)#username other-router password 123
  ```
  
  Use a syslog server
  ```
  Router(config)#logging 10.0.0.2
  ```
  
  * Only trust one port as a DHCP server for a certain vlan 
  
  ```
  Switch(config)#ip dhcp snooping
  Switch(config)#ip dhcp snooping vlan 1
  Switch(config)#int f0/3
  Switch(config-if)#ip dhcp snooping trust 
  ```
  
  * Add a virtual link
  
  ```
  router ospf 1
  area <transit_area_num> virtual-link <facing-router-id>
  ```
  
  * Activate & use RIPng (IPv6 routing)
  ```
  Router(config)#ipv6 unicast-routing 
  Router(config)#ipv6 router rip icnd2
  Router(config-rtr)#exit
  Router(config)#int g0/0
  Router(config-if)#ipv6 rip icnd2 enable 
  Router(config-if)#exit
  Router(config)#int g0/1
  Router(config-if)#ipv6 rip icnd2 enable 
  ```
  
  * Activate & use OSPFv3 (IPv6 routing)
  ```
  Router(config)#ipv6 unicast-routing 
  Router(config)#ipv6 router ospf 100
  Router(config-rtr)#router-id 1.1.1.1
  Router(config-rtr)#exit
  Router(config)#int g0/0
  Router(config-if)#ipv6 ospf 100 area 0
  Router(config-if)#exit
  Router(config)#int g0/1
  Router(config-if)#ipv6 ospf 100 area 0
  Router(config-if)#end
  ```
  
  * Activate & use EIGRPv6 (IPv6 routing)
  ```
  Router(config)#ipv6 unicast-routing 
  Router(config)#ipv6 router eigrp 100
  Router(config-rtr)#eigrp router-id 3.3.3.3
  Router(config-rtr)#no shutdown
  Router(config-rtr)#exit
  Router(config)#int g0/0
  Router(config-if)#ipv6 eigrp 100
  Router(config-if)#exit
  Router(config)#int g0/1
  Router(config-if)#ipv6 eigrp 100
  ```
  
  * Activate & use HSRP
  ```
  Router(config)#int g0/0
  Router(config-if)#ip add 10.0.0.1 255.0.0.0
  Router(config-if)#standby 1 ip 10.0.0.100
  **# Use preempt (restore the favorite if he's back)
  Router(config-if)#standby 1 preempt 
  **# Manually change the priority
  Router(config-if)#standby 1 priority 105
  **# Track this port & inform if it's down
  Router(config-if)#standby 1 track g0/1
  ```
  
  * Set command history size 
  
  ```
  Router(config)#line vty 0 15
  Router(config-line)#history size ?
    <0-256>  Size of history buffer
  ```
  
  * Set the link speed
  
  ```
  Switch(config-if)#speed ?
    10    Force 10 Mbps operation
    100   Force 100 Mbps operation
    auto  Enable AUTO speed configuration
  ```
  
  * Set the duplex mode
  
  ```
  Router(config-if)#duplex ?
    auto  Enable AUTO duplex configuration
    full  Force full duplex operation
    half  Force half-duplex operation
  ```
  
  * Set an interface as a passive interface
  
  ```
  Router(config-router)#passive-interface <interface>
  ```
  
  * Set all interfaces as a passive interface by default
  
  ```
  Router(config-router)#passive-interface  default
  ```
