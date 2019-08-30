content: '''
  # NTP
  
  * IETF 
  * 123/UDP (source and destination)
  * The system clock can obtain it's informations via:
    * Manual configuration
    * NTP
    * SNTP
    * VINES Time Service
  * NTP servers are classified based on their stratum level
    * Stratum 1: Device directly connected to a radio or atomic clock
    * Stratum 2: Device that is 1 hop from a stratum 1 device
