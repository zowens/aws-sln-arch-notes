# VPC/DirectConnect


## High Level

* Multiple VPCs per account
* Span single region
* VPC contains many subnets
* Subnets have a routing table, routing tables can be shared
* Subnets exist in a single AZ
* Private vs Public subnets:
    * Private subnets 0.0.0.0/0 traffic goes to NAT
    * Public subnets have 0.0.0.0/0 traffic through "Internet Gatway" (IGW)
* Some AWS resources can be deployed within VPC: ELB, ElastiCache, RDS, Redshift

## Subnets
* Routes exists within VPC (not modifiable)
* Subnets are a portion of the VPC address space

## Security

### NACLs
* First line of defense for packets entering VPC
* Stateless (allowed by ingress does not imply allowed by egress)
* Specified per-subnet

### Security Groups
* Specified as per-instance firewall
* Stateful (allowed by ingress implies allowed by egress)
* Can span the entire VPC
* Dynamic (can be added/removed from running instances)

## VPC Peering
* Connects 2 VPCs (can span accounts)
* Transitive connectivity not permitted
* Cannot have overlapping CIDR blocks
* Connectivity requires route in routing table with other VPC as target
* Traffic b/w VPCs is charged per GB

## VPN
* VPN attached to VPC via "Virtual Private Gateway"
* IPSec
* Needs route added to routing table for connectivity
* 2 modes: BGP or static routes (prefix)
* Hardware VPN
    * DC -> AWS
    * Automatic failover on AWS side
    * Reuses existing equipment
    * Must implement failover in DC
    * Variability in Internet conditions
    * Single-hop BGP
* Software VPN
    * Self-managed VPN inside of EC2
* Software VPN -> Hardware VPN
    * Software VPN in 1 VPC -> VGW in another VPC
* AWS VPN CloudHub
    * Hub-and-spoke model for remote office connectivity
    * Utilizes VPG within 1 VPC for redundancy 
    * The "spokes" must implement redundancy (e.g. remote offices)
* DirectConnect + VPN
    * More secure connection over DirectConnect?

## DirectConnect
* Dedicated network over private lines
* 1 to 10 GBPS
* BGP peering
* Uses VLAN 
* No redundancy, must add second connection

## ClassicLink
* Allows EC2 classic instances to obtain VPC SGs and communicate with VPC instances
* Does not become member of the VPC
* DNS does not resolve to private IP of the instance within VPC
* VPC needs to have ClassicLink enabled
* Cannot reach VPC peers, not compatible with VPG connections
* Strange restriction:
    * VPC cannot have CIDR in 10.0.0.0/8 range, except 10.0.0.0/16 and 10.1.0.0/16

## CIDR Basics
* IP address + Subnetmask
* 192.168.100.14/24
    * 24 leading 1 bits
* X.X.X.X/Y => 2^(32-Y) addresses in the mask
    * Greater the last number (mask), less IPs

## Links
[Practical VPC Design](https://medium.com/aws-activate-startup-blog/practical-vpc-design-8412e1a18dcc)
[Virtual Private Gateway](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html)
