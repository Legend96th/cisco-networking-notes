  # Syslog
  System logging
  
  * Logging levels
    0. Emergencies: Severe conditions that render a system unusable
    1. Alerts: Conditions that require immediate attention
    2. Critical: Conditions that should be addressed to prevent an interruption in service, but less severe than an Alert condition
    3. Errors: An error condition that does not render the system unusable
    4. Warnings: A condition where an operation failed to successfully complete
    5. Notifications: An administrative notification about a change to the system
    6. Information: Information about a normal system operation
    7. Debugging: Very detailed information about system operation, typically used for troubleshooting
  * The lower the level, the more severe the situation of the log message is
  * It's common to start logging at level 4 down to level 0 [[recommended]]
  * Informations in a log message 
    * Sequence number
    * Timestamp
    * Facility: The facility to which the log message refers (eg: DUAL, OSPF...)
    * Severity level
    * Mnemonic: An abreviation description of the log message
    * Description: A more detailed description of the log message
  
  
  ---
  
  # Configuration
  
  * Show console messages on console login
  ```
  Router(config)#logging console
  ```
  
  * Show console messages on remote access logins
  ```
  Router#terminal monitor
  ```
  
  * Enable logging to buffer
  ```
  Router(config)#logging buffer
  ```
  
  * Show active loggings
  ```
  Router#show logging
  ```
  
  * Configure an equipment to use a syslog server
  ```
  Router(config)#logging <syslog_server_ip_address> 
  ```
  
  * Select the logging severity level to log (the chosen one and all those more severe than it)
  ```
  Router(config)#logging trap (<0-7>|<level_name>)
  ```
  
  * Add a sequence number to the log messages
  ```
  Router#service sequence-numbers
  ```
  
  * Add a timestamp to the log messages
  ```
  Router#service timestamps log [datetime|log]
  ```
  
  * Show the logging configuration and the logging buffer
  ```
  Router#show logging
  ```
  
  
'''
tags: [
  "CISCO"
