content: '''
  # SNMP
  Simple Network Management Protocol
  A protocol used to remotely monitor, configure and received alerts from managed network devices
  
  * Components:
    * SNMP Manager (server)
      * Can query managed devices for information, set specific parameters on the managed devices and receive notifications sent by the managed devices
    * SNMP Agent (on devices):
      * A managed device that can be queried and have parameters set by an SNMP Manager and can also send notification of specific events to an SNMP Manager
      * An SNMP agent contains a MIB (Management Informatiomn Base): A hierarchical database in a managed device containing variables specific to that device, which can be provided to or configured from an SNMP Manager
        * A MIB contains a collection of OIDs (Object Identifiers): A component of a MIB containing a collection of variables, which can be queried by or configured from an SNMP Manager
          * Define what information on a managed device can be read or set from the SNMP manager
  * Information can either be:
    * Queried by the Manager
    * Sent by the agent (based on an event: time, ressource limit exceeded...etc) in the form of a Trap notification
      * A notification sent from an SNMP Agent to an SNMP Manager, which notifies the SNMP Manager about an event that occured on the managed device
  * Multiple version
    * Version 1 
      * No security
        * Uses community strings (passwords)
          * Read only community string
          * Read/Write community string
      * Monitor the CPU only
    * Version 2c 
      * Difficult to setup security (using community string)
      * Retrieve more information from a device using a single query
      * More features comparing to V1
    * Version 3  
      * More secured
        * Encryption 
        * Integrity checking
        * Authentication services
