# Scalability and Elasticity

* Relevant services
    * Elasticache
    * ELB
    * Cloudfront
    * Kinesis
    * SNS/SQS
    * Autoscaling

## Autoscaling

* Minimum/Desired/Maximum capacity settings
* Termination options
    * Longest Running
    * Oldest launch configuration
    * Closest to hour billing
* Scaling can be scheduled or manually triggered
* If whole AZ goes out, autoscaling can spin up instances in another
* Cloudwatch alarms trigger scaling when threshold crossed
* 2 types of scaling policy
    * Simple Policy
        * Locks the ASG such that no new scaling can occur
        * No re-evaluation of the metric while scaling occurs
        * Cooldown period prevents thrashing
    * Step scaling policy
        * "Instant" reaction
        * Has warmup not cooldown (?)
        * Preferred
* Lifecylce hooks 
    * Can perfom some action for the instance
    * Sent over SNS
    * Puts the instance in "wait" state until "CompleteLifecylceEvent" called or timeout
    * 2 hooks: Pending and Terminating
    * Lifecylce hooks can abort, indicating that autoscaling can terminate
* Can be attached to ELBs

## Cloudfront
* Edge locations around the world
* Use S3 for static assets
    * Origin Access Identity ensures ONLY cloudfront can access the content of S3 bucket
    * Signed URLs/Signed Cookies limit access to S3 objects from CF
* Versioning
    * Use query params/URLs for versioning
    * Set high TTL
* Low TTL for dynamic content
    * Recommendation: low TTL to serve stale content if origin unresponsive
    * Sheds origin load
* Only forward necessary headers
    * E.g. cookies
* Streaming Media
    * Manifest file should have low TTL, media files should have high TTL
    * Use HTTP streaming
        * Live streaming via EC2
        * Use signed cookies instead of signed URLs
* Use route53 for origin fallback
* Security: HTTPS end-to-end, HTTP to HTTPS redirect
* Custom SSL
    * SNI
    * Dedicated IP
    * May be used on multiple distributions
* Price classes allow limiting which distributions take traffic based on price
* Recommendation: Use route53 ALIAS records (free!) - Can be used for zone apex
* Reports can show highest hit URLs

## Elasticache
* Memcached
    * Can be cross-AZ
    * Consistent Hashing
    * Discovery endpoint for clients to discovery all nodes
* Preventing thundering herd
    * Pre-populate cache
    * Randomize TTL
    * Monitor utilization
    * Gradully scale nodes to amortize low cache hit
* Redis
    * Multi-AZ: read replicas in separate AZ
    * Snapshots on replica (?)
    * Failover uses CNAME switch
        * Can take 2 minutes
        * Java is notoriously bad at this
    * Background save is costly, can take double the memory
        * Elasticache has forkless snapshots
* Cloudwatch integration is key
