## Securing instances:</p>
The major point that has evolved in the last couple of years was how to secure the instances from being exposed and hacked. </p>
Google Identity Aware Proxy was one of the solutions. Previously we had the bastion, the latter worked for some time, now the matters,</p>
are different.

Instances come with external ephemeral IP address attached, one needs to remove that after applying google private connection.</p>

```
# Delete the external access configuration from the primary network interface
gcloud compute instances delete-access-config INSTANCE_NAME \
    --zone=YOUR_ZONE \
    --access-config-name="External NAT"
```
## Nat differences:</p>

The fundamental difference between a Public NAT Gateway and a Private NAT Gateway comes down to what IP address they translate to and where they send the traffic.
Here is a side-by-side comparison:
Feature	Public NAT Gateway	Private NAT Gateway
Translation Type	Translates private IP addresses to a public IP address.	Translates private IP addresses to another private IP address.
Target Destination	The public Internet (e.g., pulling software updates, external APIs).	Internal networks (e.g., another VPC, on-premises data center).
Internet Access	Yes (Outbound-only internet connectivity).	No (Keeps all traffic strictly off the public internet).
Primary Use Case	Allow private VMs to access the internet securely without exposing them to inbound public attacks.	Avoid IP address overlaps between connected networks (Peering/VPN) or secure multi-tenant architectures.
Detailed Breakdown
1. Public NAT Gateway
How it works: Instances without public IPs send outbound requests to the NAT Gateway. The NAT Gateway replaces the instance's private IP with its own public IP, routes the request to the internet, and routes the response back to the private instance.
Security benefit: Incoming connections from the internet are blocked by default because the private instances do not have public IPs exposed to the internet.
Example: Updating packages on a private GCP or AWS virtual machine (apt update / yum update).
