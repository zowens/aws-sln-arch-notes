# Deployment Management

## OpsWorks
* Configuration as code (Chef)
* OpsWorks for Chef Automate
    * Managed Chef server
    * Chef cookbooks are used to define node configuration
    * Chef Automate - CD pipelines, workflow, compliance
    * Node registration works via userdata from ASGs
        * Pay per node hour while connected
    * Can be used for on-premise
* OpsWorks Stacks
    * Embedded Chef solo client in EC2
    * Resource creation
        * EC2 instances from template
        * EBS volumes
        * EIP
    * Auto-healing
    * Components
        * Stack - fleet of instances
        * Layer - How to setup an instance or related resources
        * Apps
        * Deployment
        * Auto-scaling
    * Lifecycle events: setup, configure, deploy, undeploy, shutdown
    * Recipe + Metadata = Command

## Elastic Beanstalk
* AWS managed deployments of application code in specific languages
* Uses other AWS services (e.g. EC2, EBS, VPC, S3) to deliver application deployment
* Can attach ELBs
* Opt-in to platform updates (e.g. Java version updates)
* No additional charge (EC2 cost)
* Changes can be immutable, rolling, all at once, rolling w/ additional batch
* Blue/green deployments via environments

## CloudFormation

