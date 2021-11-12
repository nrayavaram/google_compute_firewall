# google_compute_firewall
Sentinel file "google_compute_firewall.sentinel" is having code to deploy the policies. In order do not allow the restricted ports, We need to validate policy successfully.**
the purpose of this policy is to validate the value of the load_balancing_scheme.
Variables :
messages: It is being used to hold the complete message of policies violation to show to the user.

control statements: here we are looping and assigning the all the resourses into two parameters

Parameters
Name	Description
address	The key inside of resource_changes section for particular GCP Resource in tfplan mock
rc	The value of address key inside of resource_changes section for particular GCP Resource in tfplan mock
condition: here the if condition is comparing the value of load_balancing_scheme is not INTERNAL_MANAGED it will generate appropriate message to show the users.

Terraform version
Terraform v1.0.7

sentinel versions
Sentinel v0.18.4

modules to import:
*import "tfplan-functions" *import "strings" *import "types"

Testing a Policy
sentinel test example : $ sentinel test google_compute_load_balancing.sentinel PASS - google_compute_load_balancing.sentinel PASS - test/google_compute_load_balancing/fail.hcl PASS - test/google_compute_load_balancing/null.hcl PASS - test/google_compute_load_balancing/success.hcl
  
