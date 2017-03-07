# High Availability and Business Continuity

## Terms
* *RTO* (Recovery Time Objective) - Interval of time where data recovery does not harm business continuity
    * E.g. How long did the recovery take from the point of loss of service until restoration? RTO is the interval of accepted lapse
* *RPO* (Recovery Point Objective)- Interval of time where data loss is acceptable for business continuity
    * E.g. if a backup was taken 1hr ago and the RPO is 2hrs, we're within RPO

## On-Prem to AWS

* Traditional methods
    * Tape - Usually WAN-replicated offsite
    * Backup to Disk
    * Virtual Tape Library - compressed, bulk data backup
* AWS Services
    * EBS-attached volumes on EC2
        * Snapshot to S3
        * Replicated within AZ
    * S3
        * 11 9's of durability
        * Lifecycle to Glacier for cold storage
    * Glacier
        * Cheap, cold storage
        * Retrieval time needs to factor into RTO!
    * Storage Gateway
        * Connects on-prem to AWS storage
        * Useful for backups/DR
        * 3 types
            * *Gateway-Cached Volumes* (cloud is primary) - Primary data in S3, local copy of hot data
            * *Gateway-Stored Volumes* (cloud is backup) - Low-latency access to entire data set, backup in S3
                * Must be provisioned up-front with local storage capable of the entire data set!
            * *Gatway Virtual Tape Library* - Backed by S3 or Glacier, iSCSI
                * Virtual Tape Library (S3)
                * ... can transition to...
                * Virtual Tape Shelf (Glacier)
    * Direct Connect
        * Low latency connection to office, co-lo or DC
    * Import/Export
        * Sometimes UPS is faster than the internet
* Storage Gateways
    * Link on-prem storage to the cloud
    * Can be over WAN, DirectConnect is probably better strategy
    * Storage Gatway is attached to S3
    * SG-backed data in S3 can be recovered on-prem or EBS
    * Security considerations
        * Data in transit is encrypte w/ SSL
        * Data is encrypted at-rest
        * Key Storage in physical location
        * Key rotation on the gateway


### FS replication
[Witepaper](https://d0.awsstatic.com/whitepapers/implementing-windows-file-server-disaster-recovery.pdf)

* Focuses on corporate data backups (elastically scaled)
* Many companies forget to test their recovery strategies (too true)
* Async replication
* Fast recovery (< 90s) using Microsoft DFS Namespaces
    * Multi-master
    * Designed for limited bandwidth networks
* May need import/export for first time based on existing data
* Keep in mind EBS volume limit (16 TB)
    * RAID or partition for >16tb 
* VPN b/w VPC and on-prem
* EBS snapshots for backups


## Cloud-native backups
* EBS snaphots to S3 (and then to Glacier)
* Hot backups (read-replicas that are promoted)
* XFS freeze -> Backup -> Unfreeze
* Multi-volume
    * Could stripe EBS volumes (Logical Volume Manager)
    * Backup at LVM layer, not device layer
* RDS
    * Automated, point-in-time backups once daily
    * User-initiated backups
* AMIs - Use EBS snapshots
* OpsWorks Auto-Healing
    * Detects failed images (after 5min)
    * Creates new image w/ similar config, attached EBS volumes, EIPs

## Cloud-native HA best practices
* Autoscaling groups for healing, scaling
* ELB for routing to ASG
* Route53 for failover
* Mutli-AZ all the things

## Multi-Region Architectures
* VPN between regions
    * Can be SPOF if not designed properly

## Links
* [Study Guide](http://blogs.catapultsystems.com/cmoore/archive/2016/01/27/aws-csa-pro-study-guide-high-availability-and-business-continuity/)

