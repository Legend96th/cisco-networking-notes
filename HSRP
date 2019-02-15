createdAt: "2018-09-12T09:15:01.347Z"
updatedAt: "2018-09-12T20:49:14.513Z"
type: "MARKDOWN_NOTE"
folder: "06e9f678163ed5819f33"
title: "HSRP"
content: '''
  # HSRP
  Hot Standby Router Protocol
  created by cisco
  
  * Hello time: 3s
  * Holddown: 10s (3 tries)
  
  
  * If we have 2 gateways in the same LAN (2 routers), we can only exist by one. If that gateway dropped we have to reconfigure all the devices in that LAN tIfo restore the connection
  * HSRP creates a virtual address to work as a gateway that can be associated to an active router, if the active router is down, one of the other passive routers take takes that address to continue serving
  * Uses priority on routers interfaces to choose which one to be the Active one; highest is chosen (def 100)
    * If the priorities are the same, the first one connected is elected as active
  * If the holddown time is passed the priority the priority is decremented (def 10) **ONLY ONCE**
  * "preemtp" is used to restore the fovorite router if it is back up
  * The port of the other end (the packet exitting port) should be Tracked to decrement the priority & inform that the route no longer exists.
    * If multiple routes exist to the destination, the decrementation happens only if all the routes are down
  
  
  
  
'''
tags: [
  "CISCO"
]
isStarred: false
isTrashed: false
