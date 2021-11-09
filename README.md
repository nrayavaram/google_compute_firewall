# google_compute_firewall
Each network has its own firewall controlling access to and from the instances.

All traffic to instances, even from other instances, is blocked by the firewall unless firewall rules are created to allow it.

The default network has automatically created firewall rules that are shown in default firewall rules. No manually created network has automatically created firewall rules except for a default "allow" rule for outgoing traffic and a default "deny" for incoming traffic. For all networks except the default network, you must create any firewall rules you need.

To get more information about Firewall, see:

##API documentation

##Example Usage - Firewall With Target Tags
-------------------------------------------


resource "google_compute_firewall" "rules" {
  project     = "my-project-name"
  name        = "my-firewall-rule"
  network     = "default"
  description = "Creates firewall rule targeting tagged instances"

  allow {
    protocol  = "tcp"
    ports     = ["80", "8080", "1000-2000"]
  }

  source_tags = ["foo"]
  target_tags = ["web"]
}



## Trigged the following messages for encountered the ports ["22" , "80" , "8080" , "3389" ]
     * Those are under restricted ports so doesn't allowed 

### The follwoing stpes are followed to check unrestricted ports,
     1.Read the mock-tfplan file fully
     2. check the resourses section 
  
