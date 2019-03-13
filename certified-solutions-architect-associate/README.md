# Certified Solutions Architect - Associate

This section of the repository aims towards _Certified Solutions Architect - Associate_ with the help from [this course](https://acloud.guru/course/aws-certified-solutions-architect-associate/dashboard) from A Cloud Guru.

## Topics for the exam

- AWS global infrastructure
- Compute
- Storage
- Databases
- Migrations
- Network and content delivery
- Management tools
- Analytics
- Security, identity & compliance
- Application integration
- Desktop & app streaming

## AWS Certified Developer - Associate

There is a path where a developer after this certification can continue with _AWS Certified Developer - Associate_. Instead of doing the complete course, the developer can focus on just the following topics.

- S3
- DynamoDB
- Application integration
- Analytics

## IAM - Identity & Access Management

- Job function policy types
  - `AdministratorAccess` - Provides full access to AWS services and resources.
  - `PowerUserAccess` - Provides full access to AWS services and resources, but does not allow management of Users and groups.
  - `SystemAdministrator` - Grants full access permissions necessary for resources required for application and development operations.
  - `NetworkAdministrator` - Grants full access permissions to AWS services and actions required to set up and configure AWS network resources.
  - `DatabaseAdministrator` - Grants full access permissions to AWS services and actions required to set up and configure AWS database services.
  - `DataScientist` - Grants permissions to AWS data analytics services.
  - `SupportUser` - This policy grants permissions to troubleshoot and resolve issues in an AWS account. This policy also enables the user to contact AWS support to create and manage cases.
  - `ViewOnlyAccess` - This policy grants permissions to view resources and basic metadata across all AWS services.
  - `SecurityAudit` - The security audit template grants access to read security configuration metadata. It is useful for software that audits the configuration of an AWS account.
  - `Billing` - Grants permissions for billing and cost management. This includes viewing account usage and viewing and modifying budgets and payment methods.
- Max 300 groups in an AWS account
- Max 1,000 roles under your AWS account
- Max 10 managed policies to a user, role, or group
- Max 1,500 customer managed policies in an AWS account
- You can only associate one IAM role with an EC2 instance
- A service-linked role is a type of role that links to an AWS service (also known as a linked service) such that only the linked service can assume the role

## S3 - Simple Storage Service

- Max 100 buckets per account
- Min file size: 0 B
- Max file size: 5 TB
- Files smaller than 128 KB will cost as 128 KB
- Consistency
  - Read after write for new objects
  - Eventual after overwrite or delete for existing objects
- Basically a key/value store
- Durability
  - 99.999999999 (11 nines)
- Availability
  - S3 Standard - 99.9% (99.99% according to course)
  - S3 Standard-Infrequent Access - 99.9%
  - S3 One Zone-Infrequent Access - 99% (99.5% according to course)
  - S3 Glacier
- S3 Reduced Redundancy Storage (RRS) - no longer recommended
  - 99.99% availability
  - 99.99% durability
- Customers are charged for
  - Storage
  - Requests
  - Storage management (tagging)
  - Data transfer
  - Transfer acceleration
- Encryption alternatives
  - Client side encryption
  - Server side encryption
    - S3 managed keys (SSE-S3)
    - AWS Key Management Service (SSE-KMS)
    - Custom provider keys (SSE-C)
- URL format: https://s3-[REGION].amazonaws.com/[BUCKET_NAME]

## Amazon Macie

- Machine Learning powered security service
- Discovers, classifies, and protects sensitive data stored in Amazon S3

## EC2 - Elastic Compute Cloud

- Options
  - On demand - Fixed rate per hour
  - Reserved - Reservation for one or three years with significant discount
    - Standard - Up to 75% off on-demand
    - Convertible - The ability to change the attributes of the EC2s
    - Scheduled - For tasks that are known in advance
  - Spot - Bidding on instance capacity
  - Dedicated hosts - Physical servers dedicated for your use
- [EC2 instance types](https://aws.amazon.com/ec2/instance-types/) (FIGHT DR MC PX)
  - F - Field Programmable Gate Array
    - Genomics research
    - Analytics
    - Video processing
  - I - High speed storage (IOPS)
    - Databases
  - G - Graphics intensive
    - Video encoding
    - 3D application streaming
  - H - High disk throughput
    - MapReduce
    - Distributed file systems
  - T - Cheap general purpose
    - Web server
    - Small databases
  - D - Dense storage
    - File servers
    - Data warehousing
  - R - Memory optimized (RAM)
    - Memory intensive apps
  - M - General purpose
    - Application servers
  - C - Compute optimized
    - CPU intensive applications and databases
  - P - Graphics
    - Machine learning
    - BitCoin mining
  - X - Memory optimized
    - SAP HANA
    - Apache Spark
- Termination Protection - Disabled by default but can be enabled in the provisioning wizard

### Gotchas when launching EC2

- Change permissions on your downloaded key-pair using the command `CHMOD 400 [NAME].pem`
- Login to the instance using the user `ec2-user`, i.e. `ssh ec2-user@[IP] -i [NAME].pem`

### Meta-data

To get the meta-data from a running EC2 instance, run the following command:

```
curl http://169.254.169.254/latest/meta-data/
```

To get the boot script from a running EC2 instance, run the following command:

```
curl http://169.254.169.254/latest/user-data/
```

### EBS - Elastic Block Storage

- A disk in the cloud that you can attach to your EC2 instances
- The root EBS volume is by default deleted when the EC2 instance is terminated
- EBS root volumes of your default AMI's cannot be encrypted
- To encrypt a EBS root volume a snapshot or image has to be created, to base the new instance from
- Additional EBS volumes can be encrypted
- Types
  - SSD
    - General Purpose SSD (GP2)
      - Balanced price and performance
      - Max 10,000 IOPS
    - Provisioned IOPS SSD (IO1)
      - Mission critical low-latency or high-throughput workloads
      - When you need more than 10,000 IOPS
  - Magnetic
    - Throughput Optimized HDD (ST1)
      - Frequent accessed or throughput-intensive workloads
      - Cannot be boot volume
    - Cold HDD (SC1)
      - Lowest cost storage for infrequent accessed workloads
      - Cannot be boot volume
    - Magnetic (Standard)
      - Lowest cost per GB for all bootable types
- Steps to create a snapshot of a RAID array, i.e. stop the application from writing to disk and flush all caches to the disk
  1. Freeze the file system
  1. Unmount the RAID array
  1. Shutting down the associated EC2 instances

### EFS - Elastic File System

- Supports the Network File System v4 (NFSv4) protocol
- Can mount the same EFS to many EC2 instances
- Only pay for what you use
- Data is stored across multiple Availability Zones
- With the same EFS mounted to many EC2 instances, one can serve the same set of files and we don't need to write complicated boot scripts to get the files on all instances
- Supports both user level permissions and directory level permissions

### AMI - Amazon Machine Images

- AMIs are regional
- Types
  - EBS
  - Instance store
    - Sometimes called "Ephemeral Storage"
    - Cannot stop instance store, only reboot or terminate
    - If the underlying host fails, you will lose your data
    - Cannot detach instance store

### Load balancers

- Types
  - Application Load Balancer (ALB)
    - Can inspect the data (on network layer 7) and route it to the correct application
  - Network Load Balancer
    - Suited for load balancing of TCP traffic (on layer 4) where extreme performance is required
  - Classic Load Balancer (ELB)
    - Can use both layer 4 and layer 7 features (such as `X-Forwarded-For` and sticky session)
    - Errors will be reported as HTTP status code `504 Gateway Timeout`
- Health check states
  - InService
  - OutOfService
- Load balancers are only exposed using DNS names, never IP addresses

### CloudWatch

- Standard monitoring in intervals of 5 minutes
- Detailed monitoring in intervals of 1 minute
- Contains the following sub components
  - Dashboards
  - Alarms
  - Events
  - Logs

### Auto-Scaling Groups

With the help of launch configurations, auto-scaling groups can provision new instances to a load balancer given failed health checks. It can also be configured to provision new instances given increased load and and tear down instances given decreased load.

Auto-scaling groups owns the EC2 instances, when a auto-scaling group i deleted so are its instances.

### Placement group

- Name must be unique in AWS account
- Types
  - Clustered placement group
    - Assume exam is talking about this type if "clustered" or "spread" isn't mentioned
    - Instances within a single Availability Zone
    - Low network latency
    - High network throughput
  - Spread placement group
    - Instances within many Availability Zones
    - Instances placed on distinct underlying hardware
    - For critical instances that should be kept separate from each other

## Lambda

- Trigger types
  - API Gateway
  - IoT
  - Alexa Skills Lit
  - Alexa Smart Home
  - CloudFront
  - CloudWatch Events
  - CloudWatch Logs
  - CodeCommit
  - Cognito Sync Trigger
  - DynamoDB
  - Kinesis
  - S3
  - SNS
- Supported languages
  - C#
  - Go
  - Java
  - Node.js
  - Python
  - Ruby
- Price model
  - Pay per invocation
    - First 1 million invocations per month are free
  - Pay on duration rounded to nearest 100ms
    - Price is based on amount of memory allocated

## Route 53

- A record - Address record
- CName - Canonical Name, resolves one domain name to another
- Alias records - Maps records sets in your hosted zone to ELB, CloudFront or S3. Always chose it over CNames.
- SOA record - Start of Authority record is information stored in a DNS zone about that zone
- Common DNS types
  - SOA record
  - NS record
  - A record
  - CNAMES
  - MX record
  - PTR record
- Routing policies
  - Simple routing
    - Default policy, often used when you have one resource that performs a given function for your domain
    - One record with many IP addresses, that are randomly returned
    - No support for health checks
  - Weighted routing
    - Lets you split your traffic based on different weights assigned
  - Latency based routing
    - Routes to the zone with the lowest latency
  - Fail over routing
    - The primary endpoint is active as long as the health check pass, and when the health check fails the traffic is routed to the secondary endpoint
  - Geo location routing
    - Route based on user geo location
    - Can specify geographic location by continent, by country, or by state in the United States
    - Priority goes to the smallest geographic region
  - Geo proximity routing
    - Route based on user geo location
    - Can use a bias in combination with a location
  - Multi-value answer routing
    - Randomly chose an endpoint among the added records, can be combined with health checks to prevent routing traffic to endpoints that aren't alive
    - Can have up to 8 healthy records in the answer

## Databases

### RDS

- OLTP (online transaction processing)
- RDS backup types
  - Automated backups
  - Database snapshots
- Default ports
  - MySQL - 3306
  - SQL Server - 1433
  - PostgreSQL - 5432
  - MariaDB - 3306
- Encryption at rest is supported for
  - MySQL
  - Oracle
  - SQL Server
  - PostgreSQL
  - MariaDB
  - Aurora
- Existing databases can be encrypted via snapshots that then are restored
- Multi-AZ
  - Allows you to have an exact copy of your production database in another Availability Zone for redundancy
  - Is for disaster recovery only
  - Available for
    - MySQL Server
    - Oracle
    - SQL Server
    - PostgreSQL
    - MariaDB
- Read replica
  - Must have automatic backup turned on
  - Can have 5 read replicas per database by default
  - Can have a read replica from a read replica in another region (inception)
  - Available for
    - MySQL Server
    - PostgreSQL
    - MariaDB
    - Aurora

### Aurora

- Managed SQL database
- Scaling
  - Start with 10 GB
  - Auto-scales in 10 GB increments to 64 TB
  - Compute can scale up to 32 vCPUs and 244 GB of memory
- 6 copies of your data spread in 3 Availability Zones
- Replica types
  - Aurora replica (currently 15)
  - MySQL read replicas (currently 5)
- Can handle loss of 2 copies of data without affecting writes
- Can handle loss of 3 copies of data without affecting reads
- Self healing

### DynamoDB

- Spread across 3 geographical data centers
  - Types
    - Eventual consistent reads (default)
      - Consistency usually reached within a second
    - Strongly consistent reads
      - If one second eventual consistency is unacceptable
- Pricing
  - Write throughput
  - Read throughput
  - Storage

### Neptune

- Fast, reliable, fully-managed graph database service
- Execute queries with open and popular graph query languages that perform well on connected data

### Redshift

- Data warehouse service in the cloud
- Example of an OLAP (online analytics processing)

### Elasticache

- Managed in-memory cache
- Types
  - Memcached
  - Redis

## Athena

- An interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL

## QuickSight

- A very fast, easy-to-use, cloud-powered business analytics service that makes it easy for all employees within an organization to build visualizations, perform ad-hoc analysis, and quickly get business insights from their data, anytime, on any device

## Glue

- Fully-managed, pay-as-you-go, extract, transform, and load (ETL) service
- Automates the time-consuming steps of data preparation for analytics

## VPC - Virtual Private Cloud

- Default 5 VPCs per account
- VPN using Virtual Private Gateway
- 1 subnet = 1 Availability Zone
- Security groups are stateful
- Network access control lists are stateless
- IP addresses
  - 10.0.0.0 - 10.255.255.255 (10/8 prefix)
  - 172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
  - 192.168.0.0 - 192.168.255.255 (192.168/16 prefix)
- To get help about CIDR, see [CIDR.xyz](https://cidr.xyz)
- VPC types
  - Default
    - All subnets have a route out to the internet
    - Each EC2 will have both a public and a private IP address
  - Custom
- VPC peering
  - Connect one VPC with another via a direct network route using private IP addresses
  - Instances behave as they are on the same private network
  - Works accross regions
- Can only have one Internet Gateway per VPC
- A custom VPC automatically creates
  - Route table
  - Network ACL
  - Security group
- 5 addresses per subnet are reserved by AWS
- NAT instance
  - Legacy solution to get EC2 instances in private subnets access to internet
  - Launched as a EC2
  - When creating it, disable source/destination check on the instance
- NAT Gateway
  - New solution to get EC2 instances in private subnets access to internet
  - Managed service
  - Automatically scales up to 10 Gbps
  - Launched from the VPC dashboard
- Flow Logs
  - Logs your traffic in your VPC, subnet or network interface to CloudWatch
  - Cannot tag a Flow Log yet
  - After a Flow Log is created, you cannot configure it

### VPC Endpoints

- Enables you to privately connect your VPC to supported AWS services
- Traffic between your VPC and the other service does not leave the Amazon network
- Endpoint types
  - Gateway Endpoints
    - Amazon S3
    - DynamoDB
  - Interface Endpoints (Powered by AWS PrivateLink)
    - The rest

### VPC Endpoint Services

- Your own application in your VPC can be configured as an AWS PrivateLink-powered service (referred to as an endpoint service)
- Other AWS principals can create a connection from their VPC to your endpoint service using an interface VPC endpoint

## SQS - Simple Queuing Service

- Max 256 KB in size
- Messages can be kept in queue from within 1 min to 14 days (4 days by default)
- Default visibility timeout is 30s, max is 12h
- Types:
  - Standard - Order is not guaranteed
  - FIFO (First-In-First-Out)
    - Order is guaranteed
    - Max 300 transactions/s
- Polling types
  - Short (default)
  - Long

## SNS - Simple Notification Service

- Subscriber types
  - HTTP
  - HTTPS
  - Email
  - Email-JSON
  - SQS
  - Application
  - Lambda
- Push notifications to
  - Apple
  - Google
  - Fire OS
  - Windows devices

## SWF - Simple Work Flow Service

- Retention period up to 1 year
- Actor types
  - Workflow starter
  - Decider - Coordination of tasks
  - Activity workers - Programs that interact with Amazon SWF to get tasks, process tasks and return result
- A task is only assigned once, and never duplicated

## SES - Simple Email Service

## Elastic Transcoder

- Media transcoder in the cloud
- Pay based on the minutes you transcode and the resolution

## API Gateway

- Has caching capabilities to improve performance

## Kinesis

- Kinesis Streams
  - Producers stream data into Streams
  - The data is retained in Streams default for 1 day
  - The retention can be increased to 7 days
  - The data is stored in shards
  - Consumers read the data from Streams
  - A shard can support
    - Max 5 transactions per second for reads
    - Max 2 MB/s for reads
    - Max 1000 records/s for writes
    - Max 1 MB/s for writes
- Kinesis Firehose
  - Producers stream data into Firehose
  - Has no shards
  - Has no retention time
  - Optional to invoke Lambda or something on the data
  - Passes the data on to S3
- Kinesis Analytics
  - SQL queries you can run on your Kinesis Streams or Kinesis Firehose
  - Can store the result in
    - S3
    - Redshift
    - Elasticsearch Cluster

## Organizations

- Consolidate multiple accounts
- Consolidated billing
  - Can save money due to volume discounts
- By default 20 accounts can be linked
- SCP - Service Control Policies

## Tags

- Tags are case sensitive
- Resource groups are grouped on tags
- Resource group types
  - Classic resource groups
    - Global
  - AWS system manager
    - Per region

## Direct connect

- A dedicated network connection from on premises to AWS
- Benefits
  - Reduces cost when using large volumes of traffic
  - Increased reliability
  - Increase bandwidth
  - Available in
    - 10 Gbps
    - 1 Gbps
    - Sub 1 Gbps can be purchased through partners

## STS - Security Token Service

- Sources
  - Federation
  - Federation with mobile apps
  - Cross account access
- Terms
  - Federation - Join lists of users from two domains
  - Identity broker - Identity from one domain federated into another domain
  - Identity store - E.g. AD, Facebook, Google
  - Identities - User of a service, e.g. Facebook

## Workspaces

- Cloud-based replacement for traditional desktops
- Windows 7
- No AWS account needed
- By default you are given local administrator access
- D:\ is backed up every 12h

## ECS

- Soft limits
  - 1000 clusters/region
  - 1000 instances/cluster
  - 500 services/cluster
- Hard limits
  - One load balancer per service
  - 1000 tasks per service (the "desired" count)
  - Max 10 containers per task definition
  - Max 10 tasks per instance
- Scheduler
  - Service scheduler
  - Custom scheduler

## Elastic Beanstalk

- High-level deployment tool that helps you get an app from your desktop to the web in a matter of minutes
- Handles the details of your hosting environment—capacity provisioning, load balancing, scaling, and application health monitoring

## Lightsail

- Dummed down version of Elastic Beanstalk
- Templates for websites, blogs, e-commerce sites, simple software and more.

## OpsWorks

- Manages applications and servers on AWS and on-premise
- Supports Chef recipes and Puppet
- A higher level service, in contrast to Cloudformation, that focuses on providing highly productive and reliable DevOps experiences for IT administrators and ops-minded developers
- 40 Stacks by default

## Key Management Service

- Managed service that enables you to easily encrypt your data

## Systems Manager parameter store

- Centralized store to manage your configuration data, whether plain-text data such as database strings or secrets such as passwords
- Builds on top of Key Management Service

## Web application firewall (WAF)

- Rules based on
  - IP addresses
  - HTTP headers
  - HTTP body
  - URI strings
  - SQL injection
  - Cross-site scripting (XSS)
- Supports
  - Amazon CloudFront
  - Application Load Balancer
- Cost is based on
  - The number of web access control lists (web ACLs)
  - The number of rules that you add per web ACL
  - The number of web requests that you receive

## Shield

- DDoS attack protection
- Standard mode (layer 3 and 4) is automatically enabled to all AWS customers at no additional cost
- Advanced mode (layer 7) provides additional protections against more sophisticated and larger attacks ($3000/month)
- Advance supports
  - Amazon EC2
  - Elastic Load Balancing
  - Amazon CloudFront
  - AWS Global Accelerator
  - Route 53

## Whitepaper - Architecting for the cloud best practices

- [Link](https://d0.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf)
- Elasticity
  - Proactive cyclic scaling
  - Proactive event based scaling
  - Auto-scaling based on demand

## Whitepaper - AWS well-architected

- [Link](https://aws.amazon.com/architecture/well-architected/)
- Pillar 1 - Security
  - Four areas
    - Data protection
    - Privilege management
    - Infrastructure protection
    - Detective controls
- Pillar 2 - Reliability
  - Three areas
    - Foundations
    - Change management
    - Failure management
- Pillar 3 - Performance efficiency
  - Four areas
    - Compute
    - Storage
    - Database
    - Space-time trade-off
- Pillar 4 - Cost optimization
  - Matched supply and demand
  - Cost-effective resources
  - Expenditure awareness
  - Optimizing over time
- Pillar 5 - Operational excellence
  - Three areas
    - Preparation
    - Operation
    - Responses

## Before taking the exam

### FAQ

- [x] [IAM FAQ](https://aws.amazon.com/iam/faqs)
- [x] [S3 FAQ](https://aws.amazon.com/s3/faqs/)
  - [x] [Request Rate and Performance Guidelines](https://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html)
- [x] [Amazon Macie FAQ](https://aws.amazon.com/macie/faq/)
- [x] [Glacier FAQ](https://aws.amazon.com/glacier/faqs/)
- [x] [RDS FAQ](https://aws.amazon.com/rds/faqs/)
- [x] [Aurora FAQ](https://aws.amazon.com/rds/aurora/faqs/)
- [x] [DynamoDB FAQ](https://aws.amazon.com/dynamodb/faqs/)
- [x] [Athena FAQ](https://aws.amazon.com/athena/faqs/)
- [x] [QuickSight FAQ](https://aws.amazon.com/quicksight/resources/faqs/)
- [x] [VPC FAQ](https://aws.amazon.com/vpc/faqs/)
  - [x] [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [x] [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
- [x] [Route 53 FAQ](https://aws.amazon.com/route53/faqs/)
- [x] [EC2 FAQ](https://aws.amazon.com/ec2/faqs/)
  - [x] [Scheduled Scaling for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/schedule_time.html)
  - [x] [Simple and Step Scaling Policies for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-simple-step.html)
  - [x] [Target Tracking Scaling Policies for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html)
  - [x] [Dynamic Scaling for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scale-based-on-demand.html)
- [x] [ELB FAQ](https://aws.amazon.com/elasticloadbalancing/faqs/)
- [x] [SNS FAQ](https://aws.amazon.com/sns/faqs/)
- [x] [SQS FAQ](https://aws.amazon.com/sqs/faqs/)
- [x] [ECS FAQ](https://aws.amazon.com/ecs/faqs/)
- [x] [Elastic Beanstalk FAQ](https://aws.amazon.com/elasticbeanstalk/faqs/)
- [x] [Lightsail FAQ](https://aws.amazon.com/lightsail/faq/)
- [x] [Lambda FAQ](https://aws.amazon.com/lambda/faqs/)
- [x] [CloudFront FAQ](https://aws.amazon.com/cloudfront/faqs/)
- [x] [Kinesis Streams FAQ](https://aws.amazon.com/kinesis/data-streams/faqs/)
- [x] [Kinesis Firehose FAQ](https://aws.amazon.com/kinesis/data-firehose/faqs/)
- [x] [SWF FAQ](https://aws.amazon.com/swf/faqs/)
- [x] [CloudWatch FAQ](https://aws.amazon.com/cloudwatch/faqs/)
- [x] [Neptune FAQ](https://aws.amazon.com/neptune/faqs/)
- [x] [Glue FAQ](https://aws.amazon.com/glue/faqs/)
- [x] [OpsWorks Stacks FAQ](https://aws.amazon.com/opsworks/stacks/faqs/)
- [x] [Shield FAQ](https://aws.amazon.com/shield/faqs/)
- [x] [WAF FAQ](https://aws.amazon.com/waf/faq/)
- [x] [Key Management Service FAQ](https://aws.amazon.com/kms/faqs/)
- [x] [Secrets Manager FAQ](https://aws.amazon.com/secrets-manager/faqs/)
- [x] [Systems Manager Parameter Store FAQ](https://aws.amazon.com/systems-manager/faq/)

### Misc

- [x] Know RPO (Recovery Point Objective) and RTO (Recovery Time Objective)
- [x] DAX (Amazon DynamoDB Accelerator)
- [ ] [VPN Connections](https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html)
- [ ] [AWS Certification Preparation Guide](https://acloud.guru/learn/aws-certification-preparation)
- [x] Knowing certain limits (for eg max size of an item in dynamoDB (400 KB) or max timeout for a Lambda function to execute (15 min) etc) is important
- [x] Understand what should go into security group (of a private subnet) when you want to allow traffic only from a particular ELB / Web tier
- [x] Build a VPC with a private and public subnet, all according to the lectures
- [x] Check the resources of the chapter `Thank you all my student`, it contains a link to a prep guide
- [x] [EBS Creating Snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-snapshot.html)
- [x] [EBS RAID Array Snapshot](https://aws.amazon.com/premiumsupport/knowledge-center/snapshot-ebs-raid-array/)
- [x] [Whizlabs practice tests](https://www.whizlabs.com/aws-solutions-architect-associate/)
- [ ] [S3 Masterclass](https://acloud.guru/learn/s3-masterclass)
