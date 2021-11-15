# google_compute_firewall
### Sentinel file "google_compute_firewall.sentinel" is having code to deploy the policies. In order do not allow the restricted ports, We need to validate  policy successfully.**

* the purpose of this policy is to avoid the restricted ports.

#### Variables :

* restrictedPorts: It is being used locally to have information of the ports which are not allowed.
* messages: It is being used to hold the complete message of policies violation to show to the user.
* fwRulesMap : it is being used to append all available ports.

#### methods :

* fwResources: This is the function, being used to get the all resourses regarding to "firewall".
##### the code :
* finding and assigning the all available ports into the ports object.

for fwResources as address,rc {
    fwRulesMap[address] = []
    allowRules = plan.evaluate_attribute(rc,"allow")
    for allowRules as eachAllow {
        ports = plan.evaluate_attribute(eachAllow,"ports")
        for ports as eachPort {
            append(fwRulesMap[address],eachPort)
        }   
    }
}

* comparing each and every avilable port with the list of restricted ports. if any port is matching with restricted ports then it will generate a voilating message to show the user


for fwRulesMap as address , ports {
    for ports as eachPort {
        #print(eachPort)
        if eachPort in restrictedPorts {
            message = eachPort + " is not allowed"
            if address in keys(messages) {
                append(messages[address],message)
            } else {
                messages[address] = [message]
            }
        }
        if (length(eachPort)) > 4 {
             Port_Range = strings.split(eachPort,"-")
             #print(Port_Range[0])
             init_value=int(Port_Range[0])
             #print(init_value)
             last_value=int(Port_Range[1])
             r_values=(range(init_value,last_value))
             #print(r_values)
             for restrictedPorts as j{
                #print("r_values: "+plan.to_string(r_values))
                #print("j: "+plan.to_string(j))
                if (int(j)) in r_values{
                    message =(string(j)+" the port number is in between the restricted port range "+string(init_value)+"-"+string(last_value)+" port not allowed")
                    if address in keys(messages) {
                    append(messages[address],message)
                    } else {
                       messages[address] = [message]
                          }}}        

               
             }
    }
}


#### Terraform version
* Terraform v1.0.7

#### sentinel versions
* Sentinel v0.18.4



modules to import:
------------------
* import "tfplan-functions"
* import "strings"
* import "types"


#### Testing a Policy
     sentinel test <sentinel file>

example :
$  sentinel test google_secret_manager_secret.sentinel

  PASS - google_secret_manager_secret.sentinel

  PASS - test/google_secret_manager_secret/fail.hcl

  PASS - test/google_secret_manager_secret/null.hcl
  
  PASS - test/google_secret_manager_secret/pass.hcl
