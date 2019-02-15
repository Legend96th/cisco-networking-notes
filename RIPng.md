createdAt: "2018-09-12T08:29:47.640Z"
updatedAt: "2018-09-12T08:43:40.030Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "RIPng"
content: '''
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
tags: []
isStarred: false
isTrashed: false
