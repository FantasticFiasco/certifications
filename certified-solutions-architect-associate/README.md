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

## S3 - Simple Storage Service

- Max file size: 5 TB
- Consistency
  - Read after write for new objects
  - Eventual after overwrite or delete for existing objects
- Basically a key/value store
- 99.99% availability
- 99.999999999 (11 nines) durability
- S3 Reduced Redundancy Storage (RRS)
  - 99.99% availability
  - 99.99% durability
- Customers are charged for:
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
- URL format: https://s3-<REGION>.amazonaws.com/<BUCKET_NAME>

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

### AMI

- AMIs are regional
- Types
  - EBS
  - Instance store
    - Sometimes called "Ephemeral Storage"
    - Cannot stop instance store, only reboot or terminate
    - If the underlying host fails, you will lose your data
    - Cannot detach instance store

### Gotchas when launching EC2

- Change permissions on your downloaded key-pair using the command `CHMOD 400 <NAME>.pem`
- Login to the instance using the user `ec2-user`, i.e. `ssh ec2-user@<IP> -i <NAME>.pem`

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

### Meta-data

To get the meta-data from a running EC2 instance, run the following command:

```
curl http://169.254.169.254/latest/meta-data/
```

To get the boot script from a running EC2 instance, run the following command:

```
curl http://169.254.169.254/latest/user-data/
```

### Auto-Scaling Groups

With the help of launch configurations, auto-scaling groups can provision new instances to a load balancer given failed health checks. It can also be configured to provision new instances given increased load and and tear down instances given decreased load.

Auto-scaling groups owns the EC2 instances, when a auto-scaling group i deleted so are its instances.

### Placement group

- Types
  - Name must be unique in AWS account
  - Clustered placement group
    - Assume exam is talking about this type if "clustered" or "spread" isn't mentioned
    - Instances within a single Availability Zone
    - Low network latency
    - High network throughput
  - Spread placement group
    - Instances within many Availability Zones
    - Instances placed on distinct underlying hardware
    - For critical instances that should be kept separate from each other

### Lambda

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
  - Multi-value answer routing
    - Randomly chose an endpoint among the added records, can be combined with health checks to prevent routing traffic to endpoints that aren't alive
    - Can have up to 8 healthy records in the answer

## Databases

### RDS

- OLTP (online transaction processing)
- RDS backup types
  - Automated backups
  - Database snapshots
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

#### Aurora

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

### Redshift

- Data warehouse service in the cloud
- Example of an OLAP (online analytics processing)

### Elasticache

- Managed in-memory cache
- Types
  - Memcached
  - Redis

## VPC

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
  - Logs you log traffic in your VPC, subnet or network interface to CloudWatch
  - Cannot tag a Flow Log yet
  - After a Flow Log is created, you cannot configure it

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

## SWF - Simple Work Flow Service

- Retention period up to 1 year
- Actor types
  - Workflow starter
  - Decider - Coordination of tasks
  - Activity workers - Programs that interact with Amazon SWF to get tasks, process tasks and return result
- A task is only assigned once, and never duplicated

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

- Read the IAM FAQ
- Read the S3 FAQ
- Read the EC2 FAQ
- Read the ELB FAQ
- Read the VPC FAQ
- Read the CloudFront FAQ
- Read the ECS FAQ
- Read the Kinesis Streams FAQ
- Read the Kinesis Firehose FAQ
- Read the RDS FAQ
- Read the SNS FAQ
- Read the Lambda FAQ
- Read the SWF FAQ
- Read the DynamoDB FAQ
- Read the SQS FAQ
- Read the CloudWatch FAQ
- Build a VPC with a private and public subnet, all according to the lectures
