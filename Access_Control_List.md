  # Access Control List
  
  * Named
    * Standard
    * Extended
  * Numbered
    * Standard (1-99)
    * Extended (100-199)
  
  ---
  
  # Numbered
  
  ## Standard
  To create a standard numbered ACL, give it an id from 1 to 99
  * Create a policy in general conf
    * Policies are applied sequentially
    * A policy "n" cant overwrite a policy "n-1" 
      * Same logic with react router picking on routes
      * Make special policies first then genral ones in the end
    * There are 2 types of policies
      * Deny: block traffic
      * Permit: allow traffic
    * If we start with deny we MUST allow otherwise everything is denied
    * Specify a network address (with mac) or use keyword "**any**"
  * Apply a policy on an interface
    * Apply a policy using it's ID
    * Specify traffic flow ("in" or "out") of the router
  
  
'''
