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
- EC2 instance types (FIGHT DR MC PX)
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
    - Can use both layer 4 and layer 7 features (such as `X-Forwarded-For` and stick session)
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

## Before taking the exam

- Read the S3 FAQ
- Read the EC2 FAQ
- Read the ELB FAQ
