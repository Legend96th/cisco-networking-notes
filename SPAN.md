content: '''
  # SPAN
  Duplicates packets from specefied ports or VLANs on the current switch & send a copy to a specified port which could contain a server to treat them
  
  
  # RSPAN
  Same as SPAN but works on not direct switch but all the switches
  
  ---
  # Configuration
  
  * SAPN configuration
  ```
  # Create a SPAN session
  ## Specify a SPAN source
  Switch(config)#monitor session <session_id> source (interface <interface>|vlan <vlan_id>|remote ...) [,<interface>|-<interface>] (rx|tx|both) # rx: received, tx: transmitted, both: tx+rx
  ## Specify a SPAN destination
  Switch(config)#monitor session <session_id> destination interface <interface>
  ```
  
  * RSPAN configuration
  
  
  
  * Show configured SPAN sessions
  ```
