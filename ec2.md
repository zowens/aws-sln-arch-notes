# EC2
* Instance types vary CPU, mem, storage, network, GPU
* Keypairs for SSH

## AMIs
* Templates for an instance
* Can be S3 or EBS backed
* EBS-backed
    * Created when the instance is stopped and EBS volume exists
    * Launched instances use EBS
* S3-backed
    * Instance store for the root volume
    * Launched instances use instance store (limits instance types that can be used)
* EBS recommended: faster boot, persistent storage

## Placement Groups
* Logical grouping within AZ
* Lower network latency, higher throughput
* Insufficient capacity error when it is not possible to launch instance in PG
* Best practice: launch all instances at once
* Instance type restrictions
* PGs can span peered VPCs, but will not get full bandwidth
* For full bandwidth, must be addressing with private IPs
* Outside bandwidth limited to 5 GBPS

## Security
* Best practice: IAM instance profile tied to IAM role for AWS credentials
* Restrict SSH access through security groups
* Least priviledge: only give access that is required
* Disable password-based logins

## Storage
* [EBS](datastorage.md#EBS)
* When instance is stopped and EBS is still attached, instance can be started again
* When instance stopped, EBS typically deleted unless deleteOnTermination=false
* Instance store
    * Multiple can be attached depending on instance type
    * Data deleted once terminated
* EFS - multiple EC2s attached to NFSv4 volume

## Networking
* [VPC](vpc.md)
* Private IPv4 space, optional IPv6
    * Secondary IP addresses are additional private IP addresses on an instance
    * Public IP is mapped to private IP via NAT
    * Subnet must enable public traffic to get a public IP
* DNS
    * Resolves public DNS address to private within VPC, public outside
* Public IP changes when stopped/terminated (public pool)

### EIP
* Static public IP associated to your AWS account
* Only for IPv4
* Can be moved b/w instances
* More $$$ for not being attached
* EC2-Classic pool != EC2-VPC pool
    * Migration to VPC
* Changed public IP of the instance

### ENI
* Elastic Network Interface
    * Private IP, secondary private IPs
    * One EIP per private IPv4 address
    * MAC address
    * SGs
* Movable to another instance (even live)
* Multiple subnets for 1 instance, in same AZ
* Can be remapped during failure (HA)

## EC2 System Manager
* Run command, inventory, state management, maintinance/deployment automation
* SSM agent on each machine

## Pricing
[See Pricing Section](costing.md)

## EBS
[See EBS section of datastorage](datastorage.md#EBS)
