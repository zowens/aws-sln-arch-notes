# Security

## High Level
* Shared security model:
    - AWS takes care of physical security, compute infrastructure, storage, etc.
    - You take care of data encryption, permissions, network configuration
* OS patches, firewall, etc. taken care of for you w/ managed services 
* IaaS is customer's responsibility (e.g. customer responsible for guest OS)
* Physcial Security: controlled access, all disks are shreaded, etc.

## Network Security
* Firewall/Boundary devices controlled by ACLs set by Amazon
* All APIs are strictly controlled
* VPC/VPN of course, huge part of security posture
* AWS network separate from corporate network... good to know :)
* Amazon deploys extensive ingress and egress monitoring globally, for DDoS protection
* Amazon restricts EC2 network traffic in accordance to Terms of Use (violations include port scanning)
* Hypervisor prevents malicious network traffic, including spoofed IP/MAC and packet sniffing
* Security groups provide VM-level firewall on EC2

## Account Security
* Root user
* MFA for root or other users
* Access keys for APIs
* RSA keypairs for SSH and CloudFront signed URLs (X.509 certs for S3 API access)
* Use IAM roles instead of embedding access keys
* Use CloudTrail/Cloudwatch Logs

## Services 

### EBS security
* Device shows up a wiped block device before reuse
* Encrypted volumes and snapshots using AES-256

### ELB
* Handles encryption/decryption of traffic
* DDoS protection
* Follows VPC security model (security groups)
* Can choose stronger ciphers for compliance reasons (SOX, PCI)
* ELB Access logs contain all requests

### VPC security
* Public/private subnets
* VPN access
* DirectConnect uses 802.1q VLANs using collocated racks in DirectConnect locations

### S3 + Glacier
* Encryption at rest using Amazon's SSE key OR KMS key (both use AES-256)
* Encryption keys stored on different hosts
* Policies allow SSE to be a requirement, if necessary
* Metadata stored in clear-text
* MFA delete - requires MFA device to delete objects from S3 (e.g. audit logs)
* Glacier encrypts archive data by default


## Links

[Overview Whitepaper](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf)
