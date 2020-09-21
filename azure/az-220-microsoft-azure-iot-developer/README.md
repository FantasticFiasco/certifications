# AZ-220: Microsoft Azure IoT Developer

[![alt text](./badge.png "Microsoft Certified: Azure IoT Developer Specialty")](https://www.youracclaim.com/badges/0cdc97fd-bb7d-4c91-b067-8c94439a5607/public_url)

This section of the repository aims towards _AZ-220: Microsoft Azure IoT Developer_ with the help from [the documentation](https://docs.microsoft.com/en-us/azure/iot-fundamentals/) from Microsoft.

## Status

- [x] Ongoing
- [x] [Passed](https://www.youracclaim.com/badges/0cdc97fd-bb7d-4c91-b067-8c94439a5607/public_url)

## IoT Hub

- [Choose the right IoT Hub tier for your solution](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-scaling)
- [Reference - choose a communication protocol](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-protocols)
  - MQTT/MQTT over WebSocket - Use on all devices that do not require to connect multiple devices (each with its own per-device credentials) over the same TLS connection
  - AMQP/AMQP over WebSocket - Use on field and cloud gateways to take advantage of connection multiplexing across devices
  - HTTPS - Use for devices that cannot support other protocols
- [File upload](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-file-upload) - A device can upload a file to an associated Azure Storage account. The process starts with a HTTP POST to `/devices/{deviceId}/files` which returns a SAS token. The token is used when sending the file, and the process is finished with a HTTP POST to `/devices/{deviceId}/files/notifications`.
- Tiers
  - Basic - Only supports telemetry, nothing else
  - Standard - Supports all features, even IoT Edge
  - Message throughput (per unit)
    - 1
      - ~1 MB/minute (1.5 GB/day)
      - 278 messages/minute (400,000 messages/day)
    - 2
      - 16 MB/minute (22.8 GB/day)
      - 4,167 messages/minute (6 million messages/day)
    - 3
      - 814 MB/minute (1144.4 GB/day)
      - 208,333 messages/minute (300 million messages/day)
- Auto-scaling - Does not exist, implement your own custom solution using e.g. Azure Functions
- Message routing
  - Default endpoint is called `messages/events` and is compatible with Event Hubs
  - Custom endpoints
    - Azure Storage
      - Blob Storage
      - Data Lake Storage Gen2 (ADLS Gen2)
    - Service Bus Queues/Topics
    - Event Hubs
- Message enrichment - Can simplify downstream processing
  - Consists of the following elements
    - Enrichment key
    - Value - A static string, `$iothubname`, `$twin.tags`, `$twin.properties.desired` or `$twin.properties.reported`
    - One or more endpoints
  - Message can come from
    - Telemetry
    - Device twin change notifications
    - Device life-cycle events
- Message cost
  - Each message is split into blocks of 4 KB, i.e. a message of size 20 KB will be split into 5 blocks and paid for individually
- Diagnostic that can be sent to Azure Monitor
  - Connections - Tracks device connect and disconnect events from an IoT hub as well as errors. This category is useful for identifying unauthorized connection attempts and or alerting when you lose connection to devices.
  - DeviceTelemetry - Tracks errors that occur at the IoT hub and are related to the telemetry pipeline. This category includes errors that occur when sending telemetry events (such as throttling) and receiving telemetry events (such as unauthorized reader).
  - C2DCommands - Tracks errors that occur at the IoT hub and are related to the cloud-to-device message pipeline
  - DeviceIdentityOperations - Tracks errors that occur when you attempt to create, update, or delete an entry in your IoT hub's identity registry. Tracking this category is useful for provisioning scenarios.
  - FileUploadOperations - Tracks errors that occur at the IoT hub and are related to file upload functionality
  - Routes - Tracks errors that occur during message route evaluation and endpoint health as perceived by IoT Hub
  - D2CTwinOperations - Tracks device-initiated events on device twins. These operations can include get twin, update reported properties, and subscribe to desired properties.
  - C2DTwinOperations - Tracks service-initiated events on device twins. These operations can include get twin, update or replace tags, and update or replace desired properties.
  - TwinQueries - Reports on query requests for device twins that are initiated in the cloud
  - JobsOperations - Reports on job requests to update device twins or invoke direct methods on multiple devices. These requests are initiated in the cloud.
  - DirectMethods - Tracks request-response interactions sent to individual devices. These requests are initiated in the cloud.
  - DistributedTracing - Tracks the correlation IDs for messages that carry the trace context header. To fully enable these logs, client-side code must be updated by following [Analyze and diagnose IoT applications end-to-end with IoT Hub distributed tracing (preview)](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-distributed-tracing).
  - Configurations - Track events and error for the Automatic Device Management feature set
  - DeviceStreams - Tracks request-response interactions sent to individual devices
  - AllMetrics - Tracks everything

### IoT Edge

- Supported runtime targets
  - Linux
  - Windows
- Installation instructions
  - Windows
    1. From Azure IoT Hub, create an IoT Edge device
    1. From an elevated PowerShell prompt, run the command `. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Deploy-IoTEdge`
    1. From an elevated PowerShell prompt, run the command `. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Initialize-IoTEdge`
    1. When prompted, provide the device connection string from the first step
  - Linux (this example uses Ubuntu Server 18.04)
    1. `curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list`
    1. `sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/`
    1. `curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg`
    1. `sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/`
    1. `sudo apt-get update`
    1. `sudo apt-get install moby-engine moby-cli iotedge`

### Automatic IoT device and module management

- Its configuration has three parts
  - Target condition - Defines the scope of device twins or module twins to be updated. It is specified as a query on twin tags and/or reported properties.
  - Target content - Defines the desired properties to be added or updated in the targeted device twins or module twins. It includes a path to the section of desired properties to be changed.
  - Metrics - Define the summary counts of various configuration states such as *Success*, *In Progress*, and *Error*

## Device Provisioning Service

- Automated re-provisioning support
  - Factory reset - The device twin data for the new IoT hub is populated from the enrollment list instead of the old IoT hub. This is common for factory reset scenarios as well as leased device scenarios.
  - Migration - Device twin data is moved from the old IoT hub to the new IoT hub. This is common for scenarios in which a device is moving between geographies.
- Enrollment-level allocation rules - Fine-grain control over how their devices are assigned to the proper IoT hub
- Custom allocation logic - Triggers an Azure Function to determine where a device ought to go and what configuration should be applied to the device

## IoT Central

- Custom device templates
  - Telemetry - Data sent as events
  - Property - Properties are read-only by default, but can be configured to be writable
  - Commands - Commands are either synchronous or asynchronous. A synchronous command must execute within 30 seconds by default, and the device must be connected when the command arrives. Use asynchronous commands for long-running operations. The device sends progress information using telemetry messages. These progress messages have the following header properties: `iothub-command-name`, `iothub-command-request-id`, `iothub-interface-id` & `iothub-command-statuscode`
  - Cloud properties - Cloud properties are part of the device template, but aren't part of the DCM. Cloud properties let the solution developer specify any device metadata to store in the IoT Central application.
- Connect devices that use SAS tokens without registering
  1. Copy the group primary key from the enrollment group
  1. Generate device credentials based on primary key
  1. Flash the device with its id and generated credentials
  1. Connect the device to the IoT Hub
  1. Migrate the device to the correct template
- Export data to
  - Event Hubs
  - Service Bus
  - Blob storage

## Azure Stream Analytics

- Supported source types
  - Event Hubs
  - IoT Hub
  - Blob storage
- Supported output types
  - Blob storage
  - Data Lake Storage Gen 1/2
  - SQL Database
  - Table storage
  - Cosmos DB
  - Power BI
  - Event Hubs
  - Service Bus queues/topics
  - Functions
  - Synapse Analytics
- Window types
  - Tumbling - Are used to segment a data stream into distinct time segments. The key differentiators are that they repeat, do not overlap, and an event cannot belong to more than one tumbling window.
  - Hopping - It hops forward in time by a fixed period. It may be easy to think of them as tumbling windows that can overlap, so events can belong to more than one hopping window result set.
  - Sliding - It outputs events only for points in time when the content of the window actually changes (when an event enters or exits the window). Every window has at least one event, like in the case of hopping windows, events can belong to more than one sliding window.
  - Session - It groups events that arrive at similar times, filtering out periods of time where there is no data. It has three main parameters: *timeout*, *maximum duration*, and *partitioning key* (optional).

## Time Series Insights

- Supported source types
  - IoT Hub
  - Event Hub
- Add an IoT hub event source to your Azure Time Series Insight environment
  1. In the IoT Hub under *Settings*, select *Built-in Endpoints*, and then select the *Events* endpoint.
  1. Under *Consumer groups*, enter a unique name for the consumer group
  1. In the Azure Time Series Insight environment under *Settings*, select *Event Sources*, and then select *Add*

## Azure Cosmos DB

- Consistency levels
  - Strong - Reads are guaranteed to return the most recent committed version of an item, even across regions
  - Bounded staleness - Reads might lag behind writes by at most `K` versions of an item or by `T` time interval, whichever is reached first. Bounded staleness is frequently chosen by globally distributed applications that expect low write latencies but require total global order guarantee.
  - Session - Within a single client session reads are guaranteed to honor the consistent-prefix, monotonic reads, monotonic writes, read-your-writes, and write-follows-reads guarantees. Session consistency is the most widely used consistency level for both single region as well as globally distributed applications.
  - Consistent prefix - Guarantees that reads never see out-of-order writes
  - Eventual - There's no ordering guarantee for reads

## Power BI

- A dataset defines a connection to Azure from were data streams into Power BI
- Supported source types
  - Basically everything
