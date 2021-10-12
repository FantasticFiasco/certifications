# AZ-204: Developing Solutions for Microsoft Azure

This section of the repository aims towards _AZ-204: Developing Solutions for Microsoft Azure_ with the help from [the documentation](https://docs.microsoft.com/en-us/learn/certifications/exams/az-204) from Microsoft and [A Cloud Guru](https://acloudguru.com).

## Status

- [x] Ongoing

## Serverless

### Microsoft Power Automate

- Design-first
- Suited for ordinary users
- Abstraction over Logic Apps
- Types
  - Automated
  - Button
  - Scheduled
  - Business process

### Logic Apps

- Design-first
- Suited for developers
- Can edit the workflow using JSON

### Azure App Service web app

- TODO
  - Create
  - Enable diag
  - Deploy code
  - Configure settings
  - Auto-scale

### Azure App Service WebJobs

- Code-first
- Runs on Azure App Service
- Types
  - Continuous
  - Triggered
- Supported languages
  - PowerShell
  - Bash
  - C# (WebJobs SDK only supports C#)
  - VB.NET
  - JavaScript
  - Python
  - PHP
  - ...

### Azure Functions

- Code-first
- Supported languages
  - C#
  - Java
  - JavaScript
  - PowerShell
  - Python
  - ...
- Function timeout
  - Default: 5 min
  - Max: 10 min
  - HTTP: 2.5 min
- Horizontal scaling
  - New instance every 10 seconds
  - Max 200 instances
- Admin and Function authorization
  - `x-functions-key` header (preferred)
  - `code` query parameter
- URL format: `https://<name>.azurewebsites.net`

### Durable Functions

- Better alternative than Logic Apps for developers writing code
- Parts
  - Client - The entry point for creating an instance of a Durable Functions orchestration. They can run in response to an event from many sources.
  - Orchestrator - Describe how actions are executed, and the order in which they are run
  - Activity - Basic unit of work

## Non-serverless

### Azure App Service

- Plans
  - Free (Cores / RAM / Storage)
    - F1 - Free (60 CPU min/day / 1GB / 1GB)
    - Shared
  - Shared (Cores / RAM / Storage)
    - D1 - Shared (240 CPU min/day / 1GB / 1GB)
    - Custom domains
    - Shared
  - Basic service plan (Cores / RAM / Storage)
    - B1 (1 / 1.75GB / 10GB)
    - B2 (2 / 3.5GB / 10GB)
    - B3 (4 / 7GB / 10GB)
    - Custom domains
    - Dedicated
  - Standard service plan
    - S1 (1 / 1.75GB / 50GB)
    - S2 (2 / 3.5GB / 50GB)
    - S3 (4 / 7GB / 50GB)
    - Custom domains
    - Auto scale
    - Virtual Network connectivity
    - Dedicated
  - Premium v2 service plan
    - P1v2 (1 / 1.75GB / 250GB)
    - P2v2 (2 / 3.5GB / 250GB)
    - P3v2 (4 / 7GB / 250GB)
    - Custom domains
    - Auto scale
    - Virtual Network connectivity
    - Dedicated
  - Premium v3 service plan
    - P1v3 (2 / 8GB / 250GB)
    - P2v3 (4 / 16GB / 250GB)
    - P3v3 (8 / 32GB / 250GB)
    - Custom domains
    - Auto scale
    - Virtual Network connectivity
    - Dedicated
  - Isolated service plan
    - I1 (1 / 3.5GB / 1TB)
    - I2 (2 / 7GB / 1TB)
    - I3 (4 / 14GB / 1TB)
    - Custom domains
    - Auto scale
    - Virtual Network connectivity
    - Dedicated
  - Isolated v2 service plan
    - I1v2 (2 / 8GB / 1TB)
    - I2v2 (4 / 16GB / 1TB)
    - I3v2 (8 / 32GB / 1TB)
    - Custom domains
    - Auto scale
    - Virtual Network connectivity
    - Dedicated

### Virtual machines

- Azure Site Recovery Manager - Disaster recovery as a service to keep applications running during outages or regional failures.

### Azure Container Instances

- Container group
  - One or many containers running on the same host, sharing the same lifecycle, resources, local network and storage volumes
  - Similar to a pod in Kubernetes

## API Management

- Sometimes called _Azure API gateway_
- Use cases
  - Can serve as a common gateway for micro-services
  - Versioning
  - Transform response types
  - Enforce consistent security requirements
  - Open API schema
  - Rate limiting
  - Caching
  - Health monitoring
- URL format: `https://<name>.azure-api.net`
- Product - A collection of APIs
- Authorization
  - `Ocp-Apim-Subscription-Key` header (preferred)
  - `subscription-key` query parameter
  - Main scopes
    - All APIs
    - Single API
    - A product
- Tiers
  - Developer - Non production workloads
  - Basic
    - SLA: 99.9%
    - 1000 requests/s
    - 2 scale units
  - Standard
    - SLA: 99.9%
    - 2500 requests/s
    - 4 scale units
  - Premium
    - Multi-region deployment
    - SLA: 99.95%
    - 4000 requests/s
    - 10 scale units/region
  - Consumption
    - Pay for usage with auto-scale
    - SLA: 99.95%
    - Some policies are not available in this tier, like rate limit on key
- Policies
  - Change the behavior of an API through configuration
  - Can execute when
    - Inbound - Request is received from a client
    - Backend - Before a request is forwarded to a managed API
    - Outbound - Before a response is sent to a client
    - On-Error - When an exception is raised
  - Scope
    - Global
    - Product
    - API
    - Operation
  - Transformations
    - Convert JSON to XML
    - Convert XML to JSON
    - Find and replace string in body
    - Mask URLs in content
    - Set backend service
    - Set body
    - Set HTTP header
    - Set query string parameter
    - Rewrite URL
    - Transform XML using an XSLT
  - Client certificates checks
    - CA
    - Thumbprint
    - Subject
    - Expiration
- Caching
  - No internal cache exists in the consumption tier, use Azure Cache for Redis instead

## Cosmos DB

- Storage medium: SSD
- Architecture
  - Physical partitions
    - Max 50 GB
    - Max 10 000 RU
  - Logical partitions
    - Important to pick the right partition id
  - Without global distribution
    - 1 leader
    - 3 followers
    - Limits
  - With global distribution, in this example with 3 regions where each region has
    - 1 leader
    - 2 followers
    - 1 forwarder with the job of forwarding changes to the leaders in the two other regions
  - Global distribution and conflicts
    - Default conflict policy is to let the last write win
    - Custom conflict policies can be accomplished by registering a stored procedure
- APIs
  - SQL
  - MongoDB
  - Table
  - Cassandra
  - Gremlin
- Consistency types
  - Strong - Writes are made to all regions. Reads does always return updated data
  - Boundless staleness - Writes are made to all regions, however there is a tolerated delay to databases in regions beyond the source
  - Session - Creates consistency for the user session. Does not wait for global commits
  - Consistent prefix - Data updates are in order, but no consistency is guaranteed
  - Eventual - Data updates are out of oder, and no consistency is guaranteed
- Document properties
  - `_rid` - Unique identifier
  - `_etag` - Tag used for optimistic concurrency control
  - `_ts` - Timestamp when document was updated
  - `_self` - Addressable URI of the document
  - `id` - User defined unique name in logical partition
- Stored procedures
  - Used when writing data
  - Written in JavaScript
  - Only executed in partition leaders
- User defined functions
  - As a stored procedure, but execute in followers

## Storage Account

- Storage
  - General purpose v2 account - Recommended
  - General purpose v1 account - Legacy
  - BlockBlobStorage account - I think this is legacy
  - FileStorage account - I think this is legacy
  - BlobStorage account - Legacy
- Name constrains
  - 3-24 characters
  - Lowercase letters and numbers
  - Universally unique
- Blob
  - Types
    - Block blob
    - Append blob
    - Page blob
  - Access tiers
    - Hot
    - Cool
    - Archive - Can be re-hydrated into hot or cool, but this process can take hours
  - Moving files
    - Azure Portal
    - AzCopy
    - Azure PowerShell
    - Azure CLI
    - Programmatically
- URLs
  - Blob storage - https://<storage account>.blob.core.windows.net
  - Azure Data Lake Gen2 - https://<storage account>.dfs.core.windows.net
  - Azure Files - https://<storage account>.file.core.windows.net
  - Queue Storage - https://<storage account>.queue.core.windows.net
  - Table Storage - https://<storage account>.table.core.windows.net
- Static website
  - Container is called `$web`
  - Will be accessible at `https://<account name>.<zone name>.web.core.windows.net/<file name>`
- Queue Storage
  - Max 64 KB message size

## Messaging

- Message
  - Contains raw data
  - Produced by one component
  - Consumed by another component
  - Services
    - Azure Queue Storage
    - Azure Service Bus
- Event
  - Lightweight notification
  - Does not contain raw data
  - May reference where the data lives
  - Publisher has no expectations
  - There are often many subscribers

## Azure Queue Storage

- Use when
  - Need an audit trail of all messages that pass through the queue
  - Expect the queue to exceed 80 GB in size
  - Want to track progress for processing a message inside of the queue
- Max size 64 KB

## Azure Service Bus

- Use topics when
  - Multiple receivers should handle each message
- Use queues when
  - Need an At-Most-Once delivery guarantee
  - Need an At-Least-Once guarantee
  - Need a FIFO guarantee
  - Need to group messages into transactions
  - Want to receive messages without polling the queue
  - Need to provide a role-based access model to the queues
  - Need to handle messages larger than 64 KB but less than 256 KB
  - Queue size will not grow larger than 80 GB
  - Want to publish and consume batches of messages
- Types
  - Queues - Publish messages to one subscriber
  - Topics - Publish messages to multiple subscribers
    - Filters
      - Boolean filters
      - SQL filters
        - Most flexible
        - Computationally expensive
      - Correlation filters
  - Relays - Two-way communications between applications
- Tiers
  - Basic
    - Features
      - Queues
    - Max size 256 KB
  - Standard
    - Features
      - Queues
      - Topics
    - Max size 256 KB
  - Premium
    - Features
      - Queues
      - Topics
    - Max size 1 MB

## Azure Event Grid

- Use when
  - Simplicity
  - Filtering
  - Fan out
  - Pay er event
- Max 64 KB
- Event consists of
  - Topic
    - System
    - Custom
  - Subject
  - Id
  - Event type
  - Event time
  - Data
  - Data version
  - Metadata version
- Event sources
  - Subscription
  - Event Hub
  - Iot Hub
  - ...
- Event handlers
  - Azure Function
  - Webhook
  - Azure Logic App
  - Microsoft Power Automate

## Azure Event Hub

- Use when
  - Event ingestor
  - Stream events to storage
  - Security
  - Analytics
  - Reliability
  - Resiliency
- Partitions
  - Acts as buffers
  - Has at least 2

## Azure Resource Manager (ARM)

- Required JSON fields
  - `$schema` - Azure template version
  - `contentVersion` - User defined version, not the version of ARM
  - `resources`

## Azure Container Registry (ACR)

- Tiers
  - Basic - For development
  - Standard - More storage and throughput for production
  - Premium - Even more storage and throughput, as well as geo-replication content trust, and compatibility with Private Link

## TODO

- [ ] JSONP
- Azure Queue storage
- Azure Service Bus
- [ ] Learning paths and documentation
  - [ ] API Management
  - [ ] Service Bus
  - [ ] Event Hub
  - [ ] App Configuration
  - [ ] Microsoft Identity Platform
  - [ ] Cosmos DB
  - [ ] App Service
  - [ ] Function App
  - [ ] VM
- [ ] [Choose the right integration and automation services in Azure](https://docs.microsoft.com/en-us/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs)
- [ ] [What is Azure Logic Apps?](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview)
- [ ] [Get started with Power Automate](https://docs.microsoft.com/en-us/power-automate/getting-started)
- [ ] [Run background tasks with WebJobs in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/webjobs-create)
- [ ] [Introduction to Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview)
- [ ] [Tutorial: Create a function to integrate with Azure Logic Apps](https://docs.microsoft.com/en-us/azure/azure-functions/functions-twitter-email)
- [ ] Azure Web Apps
