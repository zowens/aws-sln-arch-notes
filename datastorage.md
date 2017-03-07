# RDS
* Managed instances within VPC of provider-specific DB Engines:
    * Aurora
    * Microsfot SQL Server
    * MySQL
    * PostgreSQL
    * MariaDB
    * Oracle
* VPC
    * DB subnets are specific to RDS
    * ClassicLink to EC2-classic
* AWS handles backups, patching, failure detection, repair
* 99.95% SLA for "uptime"
* Prices are > EC2 cost of managing yourself
    * Reserved instance prices
* Multi-AZ deployments possible/encouraged
* Instance sizing can be changed, opt-in for immediate outside of maintinance window
* EBS backed (General purpose SSD, or Provisioned IOPS)
* Backups
    * 2 types: automated backups and snapshots
    * Snapshots are user initiated
    * Automated backups are free w/ 1 day retention of daily backup
* Encryption of data at rest w/ KMS
    * CloudHSM is an option for Oracle
* Replication
    * Up to 5 read replicas 
        * MariaDB, PostgreSQL, MySql supported
        * Cross-region supported
    * Cross-AZ if configured for multiple subnets
    * Can force failover

## Aurora
* MySQL-compatible database built by Amazon
* Replicated to 6 storage nodes
* Snapshots
* Cross-region replication supported
    * Can be failover target (manual)
* Read replicas can be Aurora replicas (up to 15) or MySQL (up to 5)
* Underlying storage shared between read replicas, failover takes 60s (no replay of transactions)

# DynamoDB

...

# Redshift
* AWS-managed data warehouse (petabyte scale)
* Built for Business Intelligence, analytics
* Columnar storage w/ multiple compression schemes
* 2 types of nodes: compute and leader
    * Leader nodes act as coordinators, taking query plans and directing to compute nodes
    * Compute nodes receive queries from leaders, hold the data
* 2 types of nodes:
    * _Dense Storage_ HDD-backed storage, cheaper for large amounts of data
        * Large (2TB) and Extra Large (16TB)
    * _Dense Compute_ SSD-backed storage, faster compute w/ large amount of RAM
        * Large (160GB) and Extra Large (2.56TB)
* Failover
    * Data is continously backed up to S3
    * During failure, new nodes can be brought on, and other nodes will serve queries or pull from S3
    * 1 day of free backup in S3
* VPC
    * Can be deployed in EC2-classic or VPC
    * VPC-endpoint for S3 supported
* Security
    * Encryption-at-rest w/ KMS or CloudHSM
    * Access policies w/ IAM policy language
    
# EBS
* EBS volumes are block storage attacked to EC2 (one at a time)
* Automatically replicated within single AZ
* Snapshots
    * Stored in S3, incrimental blocks changed copied
    * First snapshot can take time
    * Can be used to change EBS volume type (although, we can now use elastic volumes)
* Volumes can be encrypted with KMS keys (snapshots also encrypted)
* Levels
    * General Purpose SSD (gp2)
        * 99.999% availability
        * 3 IOPs/GB with burst up to 3k IOPS
    * Provisioned IOPS SSD (io1)
        * Consistent performance
    * Throughput Optimized HDD (st1)
        * Througput-intensive workloads (high throughput)
        * Use for Kafka
    * Cold HDD (sc1)
        * Low access
    * EBS Magnetic (Last Generation)
        * Low performance HDD
* RAID = Redundant Array of Independent Disks
    * RAID 0 – Striped, no redundancy, good performance
    * RAID 1 – mirrored, redundancy
    * RAID 5 – good for reads, bad for writes, AWS does not recommend ever putting RAID 5’s on EBS
    * RAID 10 – Striped & Mirrored, good redundancy, good performance

# Others

* [S3/Galcier](s3.md)

## Links
[I/O Perf](http://engineering.theladders.com/2016/10/10/aws-io-performance-whats-bottlenecking-now/)
