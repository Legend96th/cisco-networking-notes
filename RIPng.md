  # RIPng
  
  RIP protocol for IPv6
  
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
  
  
'''
