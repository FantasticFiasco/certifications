# AZ-204: Developing Solutions for Microsoft Azure

This section of the repository aims towards _AZ-204: Developing Solutions for Microsoft Azure_ with the help from [the documentation](https://docs.microsoft.com/en-us/learn/certifications/exams/az-204) from Microsoft.

## Status

- [x] Ongoing

## Serverless

### Logic Apps

- Design-first
- Suited for developers
- Can edit the workflow using JSON

### Microsoft Power Automate

- Design-first
- Suited for ordinary users
- Abstraction over Logic Apps
- Types
  - Automated
  - Button
  - Scheduled
  - Business process

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

- Types
  - Client - The entry point for creating an instance of a Durable Functions orchestration. They can run in response to an event from many sources.
  - Orchestrator - Describe how actions are executed, and the order in which they are run
  - Activity - Basic unit of work
- Better alternative than Logic Apps for developers writing code

## Cosmos DB

- SSD
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

### Static website

- Container is called `$web`
- Will be accessible at `https://<account name>.<zone name>.web.core.windows.net/<file name>`

### Azure Queue Storage

- Message
  - Max 64 kB

## API Management

- Use cases
  - Can serve as a common gateway for micro-services
  - Transform response types
  - Enforce consistent security requirements
- URL format: `https://<name>.azure-api.net`
- Authorization
  - `Ocp-Apim-Subscription-Key` header with subscription key

## TODO

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
