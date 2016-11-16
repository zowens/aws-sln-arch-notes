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

## ClassicLink
* Allows EC2 classic instances to obtain VPC SGs and communicate with VPC instances
* VPC needs to have ClassicLink enabled
* Can be used with peering

## Links
[Practical VPC Design](https://medium.com/aws-activate-startup-blog/practical-vpc-design-8412e1a18dcc)
[Virtual Private Gateway](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html)
