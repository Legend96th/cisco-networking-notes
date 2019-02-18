  # Remote access
  
  
  * Check enabled protocols:
    * `show line vty <num>`
    * Check "Allowed <input|output> transports are ..."
  * Change enabled protocols:
  ```
  Router(config-line)#transport input ?
    all     All protocols
    none    No protocols
    ssh     TCP/IP SSH protocol
    telnet  TCP/IP Telnet protocol
  Router(config-line)#transport output ?
    all     All protocols
    none    No protocols
    ssh     TCP/IP SSH protocol
    telnet  TCP/IP Telnet protocol
  ```
  ---
  
  # SSH
  Secure Shell
  
  SSH uses an RSA key
  * 512 -> 767 v1
  * 768 -> ~ v2
  
  To activate SSH 
  * Add a hostname
  * Add a domain name
  * Add an RSA key
  * Set the SSH version to version 2
  * Add users with privilages & passwords
  * Add a line vty
  * Choose SSH as an input transport method
  * Set the username & password database to local
  
  ```
  hostname sw1
  ip domaine-name icnd2.com
  crypto key generate rsa  :: 1024
  username sofiane privilege 15 secret passWord
  line vty 0 4
  transport input SSH 
  login local
  
  
  Router(config)#hostname RW0
  RW0(config)#ip domain-name icnd2.com
  RW0(config)#crypto key generate RSA
  
    How many bits in the modulus [512]: 1024
  
  RW0(config)#ip ssh version 2
  RW0(config)#username hossam password cisco
  RW0(config)#username hossam privilege 15 secret 123
  RW0(config)#username said privilege 7 secret 456
  RW0(config)#line vty 0 4
  RW0(config-line)#transport input ssh
  RW0(config-line)#login local
  
  # On PC
  
  C:\\>ssh -l hossam 10.0.0.1
  ```
  
  
  ---
  
  # Telnet
  
  Enabled by setting a login password on the "line vty" & setting an enable password
'''
