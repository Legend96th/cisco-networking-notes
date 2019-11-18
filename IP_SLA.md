content: '''
  # IP SLA
  
  * Use cases
    * Measure network performance (delay, jitter...)
    * Influence routing decisions
  * How does it work:
    * Uses some tests with specified criterias sent in periodic time 
    * This set of tests and time period are bundled in an **SLA Operation** which is applied to a router
    * The router will apply the specified test on the specified periods and sends a corresponding packet to the far end router (destination router) which is configured as an SLA responder 
      * If the responder cannot be configured (not our equipment), we can use mechanisms that don't require configuration (icmp echo, ...etc)
    * The SLA responder will then reply with the test results so that we can monitor the results
  * IP SLAs cannot be edited, once they are applied they have to be removed and entered again
  
  ---
  # Configuration
  
  ```
  # Create an IP SLA operation 
  Router(config)#ip sla <ip_sla_id>
  
  # Configure the IP SLA to use icmp echo
  Router(config-ip-sla)#icmp-echo <destination_ip_address> source-ip <source_ip_address>
  
  # Set the frequency of the IP SLA tests
  Router(config-ip-sla-echo)#frequency <freq_num>
  
  # Set the threshold: the maximum value of the echo response delay 
  Router(config-ip-sla-echo)#treshold <time_in_ms>
  
  # Start an IP SLA operation
  Router(config)#ip sla schedule <ip_sla_id> life (forever|<seconds>) start-time (now|...)
  
  # Associate the IP SLA with a tracking object
  Router(config)#track <track_id> ip sla <ip_sla_id> 
  ## Prevent route flapping (continually shifting between one link and another)
  Router(config)#delay down <time_sec> up <time_sec>
  
  # Add a static route that is applied only when the track criteria is met
  Router(config)#ip route <network_add> <mask> <next_hop_id_add> track <track_id> 
  # Add a static route with an AD higher than the previous one to act as a backup route if the first route isn't applied
  Router(config)#ip route <network_add> <mask> <next_hop_id_add> <AD>
  ```
  
  * Show track information
  ```
  Router#show track <track_id>
  ```
  
  * Show the IP SLA statistics
  ```
