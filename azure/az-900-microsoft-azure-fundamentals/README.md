# AZ-900: Microsoft Azure Fundamentals

This section of the repository aims towards _AZ-900: Microsoft Azure Fundamentals_ with the help from [this course](https://learn.acloud.guru/course/az-900-microsoft-azure-fundamentals/dashboard) from A Cloud Guru.

## The Language of Cloud Computing

- Disaster recovery
  - Designated time to recovery (how long does it take to recover)
  - Recovery point (point of data to recover)

## Cloud Concepts

- Capital Expenditure (CapEx) - CapEx is the spending of money on physical infrastructure up front, and then deducting that expense from your tax bill over time. CapEx is an upfront cost, which has a value that reduces over time.
- Operational Expenditure (OpEx) - OpEx is spending money on services or products now and being billed for them now. You can deduct this expense from your tax bill in the same year. There's no upfront cost. You pay for a service or product as you use it.
- Cloud service models
  - Infrastructure as a service (IaaS) - Aims to give you complete control over the hardware that runs your application (IT infrastructure servers and virtual machines (VMs), storage, networks, and operating systems)
  - Platform as a service (PaaS) - Provides an environment for building, testing, and deploying software applications
  - Software as a service (SaaS) - Software that is centrally hosted and managed for the end customer

## Cloud architecture models

- Private cloud - You create a cloud environment in your own datacenter and provide self-service access to compute resources to users in your organization
- Public cloud - You have no local hardware to manage or keep up-to-date – everything runs on your cloud provider’s hardware
- Hybrid cloud - Combines public and private clouds, allowing you to run your applications in the most appropriate location

## Core Cloud Services - Azure architecture and service guarantees

- Region - A region is a geographical area on the planet containing at least one, but potentially multiple datacenters that are nearby and networked together with a low-latency network
Region Pairs - Each Azure region is always paired with another region within the same geography (such as US, Europe, or Asia) at least 300 miles away.
- Geography - A discrete market typically containing two or more regions that preserve data residency and compliance boundaries
- Availability Zone - At minimum 3 physically separate datacenters within an Azure region
  - Zonal services – You pin the resource to a specific zone (for example, virtual machines, managed disks, IP addresses)
  - Zone-redundant services – Platform replicates automatically across zones (for example, zone-redundant storage, SQL Database)

## Control and organize Azure resources with Azure Resource Manager (ARM)

- Resource group - A logical container for resources deployed on Azure
- Resource locks
  - Delete - Allow all operations against the resource but block the ability to delete it
  - Read-only - Only allow read activities to be performed against it, blocking any modification or deletion of the resource

## Core Cloud Services - Azure compute options

- Four common techniques
  - Virtual machines
  - Containers
  - Azure App Service
  - Serverless computing
- Virtual machines
  - Price calculated hourly
  - Scale sets
    - Scale sets are identical VMs
    - They can be activated or inactivated as needed
    - Works perfectly with a load balancer
    - Max 1000 VMs in a single scale set
- [Azure App Service](https://azure.microsoft.com/services/app-service/)
  - PaaS offering in Azure that is designed to host enterprise-grade web-oriented applications
  - You pay for the Azure compute resources your app uses while it processes requests based on the App Service Plan you choose
  - Types
    - Web apps - Full support for hosting web apps using ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP, or Python.
    - Web apps for containers
    - API apps - REST-based Web APIs using your choice of language and framework. You get full Swagger support, and the ability to package and publish your API in the Azure Marketplace.
    - WebJobs - Run a program (.exe, Java, PHP, Python or Node.js) or script (.cmd, .bat, PowerShell, or Bash) in the same context as a web app, API app, or mobile app. They can be scheduled, or run by a trigger. This is often used to run background tasks as part of your application logic.
    - Mobile Apps - Build a back-end for iOS and Android apps
- VM availability sets
  - A logical grouping of two or more VMs that help keep your application available during planned or unplanned maintenance
  - No cost for an availability set
  - With an availability set, you get
    - Up to three fault domains that each have a server rack with dedicated power and network resources
    - Five logical update domains
- Maintenance
  - Planned maintenance - When the underlying Azure fabric that hosts VMs is updated by Microsoft
  - Unplanned maintenance - A hardware failure in the data center, such as a power outage or disk failure
- [Azure Batch](https://azure.microsoft.com/services/batch/) - Enables large-scale job scheduling and compute management with the ability to scale to tens, hundreds, or thousands of VMs
- Containers
  - [Azure Container Instances (ACI)](https://azure.microsoft.com/services/container-instances/)
  - [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/)
- Serverless
  - [Azure Functions](https://azure.microsoft.com/services/functions/)
    - Commonly used when you need to perform work in response to an event, often via a REST request, timer, or message from another Azure service and when that work can be completed quickly, within seconds or less
    - Normally stateless, but Durable Functions provide state
    - Code-first (imperative)
    - Can run locally or in the cloud
  - [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
    - Executes workflows built from predefined logic blocks. They are specifically designed to automate your business processes.
    - Stateful
    - Designer-first (declarative)
    - Large collection of connectors
    - Large collection of ready-made actions
    - Runs only in the cloud

## Core Cloud Services - Azure data storage options

- [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/) - A relational database as a service (DaaS) based on the latest stable version of the Microsoft SQL Server database engine
- [Azure Cosmos DB](https://azure.microsoft.com/en-us/services/cosmos-db/) -  A globally distributed database service that supports schema-less data, letting you build highly responsive and Always On applications to support constantly changing data
- [Azure Blob storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
  - Storage for unstructured data. It has the ability to store up to 8 TB of data for virtual machines.
  - Tiers
    - Hot storage tier - Optimized for storing data that is accessed frequently
    - Cool storage tier - Optimized for data that are infrequently accessed and stored for at least 30 days
    - Archive storage tier - For data that are rarely accessed and stored for at least 180 days with flexible latency requirements
- [Azure Data Lake Storage](https://azure.microsoft.com/en-us/services/storage/data-lake-storage/) - Combines the scalability and cost benefits of object storage with the reliability and performance of the Big Data file system capabilities
- [Azure Files](https://azure.microsoft.com/en-us/services/storage/files/) - Fully managed file shares in the cloud that are accessible via the industry standard Server Message Block (SMB) protocol
- [Azure Queue Storage](https://azure.microsoft.com/en-us/services/storage/queues/) - A service for storing large numbers of messages that can be accessed from anywhere in the world
- [Azure Disk Storage](https://azure.microsoft.com/en-us/services/storage/disks/) - Provides disks for virtual machines, applications, and other services to access and use as they need, similar to how they would in on-premises scenarios
  - Encryption types
    - Azure Storage Service Encryption (SSE) - For data at rest, and helps you secure your data to meet the organization's security and regulatory compliance
    - Client-side encryption - Data is encrypted by the client libraries

## Core Cloud Services - Azure networking options

- Virtual network
  - A logically isolated network on Azure
  - Scoped to a single region
  - Multiple virtual networks from different regions can be connected together using virtual network peering
- Network Security Group (NSG) - Allows or denies inbound network traffic to your Azure resources. Think of it as a cloud-level firewall for your network.
- Availability - Refers to how long your service is up and running without interruption
- Resiliency - Refers to a system's ability to stay operational during abnormal conditions
- Azure Load Balancer
  - Supports inbound and outbound scenarios
  - Provides low latency and high throughput
  - Scales up to millions of flows for all Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) applications
- Azure Application Gateway
  - Only for HTTP(S)
  - A potentially better option is to use Azure Application Gateway
  - Supports
    - Cookie affinity - Useful when you want to keep a user session on the same backend server
    - SSL termination
    - Web application firewall (WAF)
    - URL rule-based routing
    - Rewrite HTTP headers
- Azure Traffic Manager
  - Uses the DNS server that's closest to the user to direct user traffic to a globally distributed endpoint

## Security, responsibility and trust in Azure

- Azure Security Center
  - Provide security recommendations based on your configurations, resources, and networks
  - Monitor security settings across on-premises and cloud workloads, and automatically apply required security to new services as they come online
  - Continuously monitor all your services, and perform automatic security assessments to identify potential vulnerabilities before they can be exploited
  - Use machine learning to detect and block malware from being installed on your virtual machines and services. You can also define a list of allowed applications to ensure that only the apps you validate are allowed to execute.
  - Analyze and identify potential inbound attacks, and help to investigate threats and any post-breach activity that might have occurred.
  - Provide just-in-time access control for ports, reducing your attack surface by ensuring the network only allows traffic that you require.
  - Tiers
    - Free
    - Standard - $15 per node per month
- Azure Active Directory
  - Single-Sign-On (SSO)
  - Multi-factor authentication
  - Application management - You can manage your cloud and on-premises apps using Azure AD Application Proxy, SSO, the My apps portal (also referred to as Access panel), and SaaS apps.
  - Business to business (B2B) identity services
  - Device Management
- Azure Key Vault
  - Secrets management
  - Key management
  - Certificate management
  - Store secrets backed by hardware security modules (HSMs)
- Microsoft Azure Information Protection (AIP) - A cloud-based solution that helps organizations classify and optionally protect documents and emails by applying labels

## Apply and monitor infrastructure standards with Azure Policy

- Azure Policy - A service in Azure that you use to define, assign, and, manage standards for resources in your environment
- Policy definition examples
  - Allowed Storage Account SKUs
  - Allowed Resource Type
  - Allowed Locations
  - Allowed Virtual Machine SKUs
  - Not allowed resource types
- Policy assignment - A policy definition that has been assigned to take place within a specific scope
- Policy Effects
  - Deny -The resource creation/update fails due to policy
  - Disabled - The policy rule is ignored (disabled). Often used for testing.
  - Append -Adds additional parameters/fields to the requested resource during creation or update. A common example is adding tags on resources such as Cost Center or specifying allowed IPs for a storage resource.
  - Audit, AuditIfNotExists - Creates a warning event in the activity log when evaluating a non-compliant resource, but it doesn't stop the request.
  - DeployIfNotExists - Executes a template deployment when a specific condition is met. For example, if SQL encryption is enabled on a database, then it can run a template after the DB is created to set it up a specific way.
- Initiative definition - A set or group of policy definitions to help track your compliance state for a larger goal
- Azure Management Groups - Containers for managing access, policies, and compliance across multiple Azure subscriptions
- Azure Blueprint - Defines a repeatable set of Azure resources that implement and adhere to your organization's standards, patterns, and requirements
- Microsoft Privacy Statement - Explains what personal data Microsoft processes, how Microsoft processes it, and for what purposes
- Microsoft Trust Center - A website resource containing information and details about how Microsoft implements and supports security, privacy, compliance, and transparency in all Microsoft cloud products and services
- Service Trust Portal - Hosts the Compliance Manager service, and is the Microsoft public site for publishing audit reports and other compliance-related information relevant to Microsoft’s cloud services
- Compliance Manager - A workflow-based risk assessment dashboard within the Trust Portal that enables you to track, assign, and verify your organization's regulatory compliance activities related to Microsoft professional services and Microsoft cloud services such as Office 365, Dynamics 365, and Azure
- Azure Monitor - Delivers a comprehensive solution for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments
- Application Insights - Monitors the availability, performance, and usage of your web applications, whether they're hosted in the cloud or on-premises
- Azure Monitor for containers - A service that is designed to monitor the performance of container workloads, which are deployed to managed Kubernetes clusters hosted on Azure Kubernetes Service (AKS)
- Azure Monitor for VMs - A service that monitors your Azure VMs at scale, by analyzing the performance and health of your Windows and Linux VMs (including their different processes and interconnected dependencies on other resources, and external processes)
- Azure Service Health - A suite of experiences that provide personalized guidance and support when issues with Azure services affect you
