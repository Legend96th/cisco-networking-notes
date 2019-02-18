  # NAT
  Network Address Translation
  
  Can map a private IP address to a publicly routable IP address, selected from a pool of publicly routable IP addresses
  
  2 types of addresses
  * Public
  * Private
  
  Reserved addresses (free to use):
  10.0.0.0 to 10.255.255.255
  172.16.0.0 to 173.31.255.255 
  192.168.0.0 to 192.168.255.255
  
  Sender =(1)=> Router =(2)=> ISP =()=> =()=> =()=> =()=> 
  
  NAT types
  * Static NAT
    * Translate IP addresses one to one
    * Need one public address for every local one
    * Assigned manually by the network administrator
  * Dynamic NAT
    * Translate multiple inside local addresses to multiple outside global addresses from a specified pool
    * Assigned automatically by the router
    * Need one public address for every local one
  * NAT overloading (PAT - Port Address Translation)
    * Map multiple private (inside local) IP address to a single public (inside global) IP address
    * Router uses port numbers to identify which host's address is translated
    * Could have 65000 hosts on a single public IP address 
  
'''
