content: '''
  # DHCP
  
  * Operates in the **Application layer**
  * 67-68/UDP
  * Must setup the server's address before it can work
  * Work with the **DORA** principle:  
    * Discover 
      * Client send discovery packet
      * Broadcast on L3 & L2
    * Offer
      * Server responds with a possible offer (an IP address that the client can use)
      * Before sending an offer a DHCP does the following
        * Send an ICMP echo (ping) request which forces an arp request to check if the IP address is already in use
      * Unicast on L2 & L3
    * Request
      * Client requests that IP address
      * Broadcast on L3 & L2
        * The request is sent as a broadcast to notify:
          * The server that offered the address that it's taking this address (it is now taken by this host)
          * The other servers that it didn't take their offer (they can offer it to someone else)
    * Acknoledgement
      * The offering server confirms the address
      * Unicast on L2 & L3
  * DHCP option 82: Causes a DHCP Request packet to contain information indicating the switch port and vlan from which the DHCP request came from
    * If a DHCP server is implemented on a multilayer equipment using 802.1Q (router, L3 switch, server with 802.1Q capable NIC) we don't need to send "option-82" because it already knows the source vlan of the request using the 802.1Q tag
    * If a DHCP server is using a simple access interface, we need to add "option-82" in order to track the source of the request (source vlan) in order to send a reply
  * Client ID: A unique device identifier used by DHCP to identify clients in a unique way. Different methods of calculating the Client ID exist and they differ from one constructor/platform to another. To get the client-id, let the machine connect to a global pool, use `show dhcp binding` and extract the dhcp client id
    * Some of the parameters used to calculate the Client ID are:
      * Mac address
      * Hostname
      * Username
      * Password
  
  ---
  
  # DHCP relay agent
