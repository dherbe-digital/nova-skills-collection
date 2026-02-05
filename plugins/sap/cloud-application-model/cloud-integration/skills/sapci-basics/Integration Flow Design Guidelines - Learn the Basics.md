# Integration Flow Design Guidelines - Learn the Basics

**Date:** 2026-02-05

## Overview

This comprehensive guideline covers fundamental concepts and practical examples for building integration flows in SAP Cloud Integration. It provides foundational knowledge for developers new to integration patterns and SAP Cloud Integration platform.

## Key Design Principles

- **Message-oriented architecture**: Design flows around message processing
- **Asynchronous processing**: Decouple systems using asynchronous patterns
- **Data transformation**: Convert between different data formats
- **File handling**: Manage file-based integrations
- **Attachment handling**: Process message attachments
- **Data persistence**: Store and retrieve integration data

## Modeling Basics

### Timer-Based Integration

#### Modeling Basics - Timer-Initiated Scenario
Demonstrates a flow triggered by a timer at regular intervals. Useful for scheduled batch processing.

#### Modeling Basics - Timer-Initiated Scenario with External Data Source and Receiver
Shows timer-triggered flow that fetches data from external source and sends to receiver.

### HTTP Integration

#### Modeling Basics - Consume Public HTTP Service With Query Parameters
Demonstrates how to call HTTP services with query parameters. Shows proper parameter encoding and handling.

#### Modeling Basics - Restrict Http Method
Shows how to restrict which HTTP methods (GET, POST, PUT, DELETE) are allowed on an endpoint.

### Sender-Initiated Integration

#### Modeling Basics - Sender-Initiated Scenario with External Data Source and Receiver
Demonstrates flow triggered by external sender that fetches additional data and sends to receiver.

### Data Format Conversion

#### Modeling Basics - XML To JSON Mapping
Shows how to transform XML messages to JSON format. Useful for modern API integrations.

#### Modeling Basics - JSON to XML Converter And XML to JSON Converter
Demonstrates bidirectional conversion between JSON and XML formats.

#### Modeling Basics - CSV To XML Converter
Shows how to convert CSV files to XML format for processing.

#### Modeling Basics - XML To CSV Converter
Demonstrates conversion from XML to CSV format for export scenarios.

#### Modeling Basics - Base64 Encoder
Shows how to encode data in Base64 format for safe transmission.

#### Modeling Basics - Base64 Decoder
Demonstrates decoding Base64-encoded data.

#### Modeling Basics - MIME Multipart Encoder Decoder
Shows how to handle MIME multipart messages with multiple content parts.

### Data Access & Persistence

#### Modeling Basics - Usage Of Data Store
Demonstrates how to use the data store step to persist data during flow execution.

#### Modeling Basics - Use The Persist Step
Shows how to use the persist step for temporary message storage.

#### Modeling Basics - Delta Sync With Timestamp Via Payload
Demonstrates delta synchronization using timestamp in message payload.

#### Modeling Basics - Delta Sync With Timestamp Via Date Now
Shows delta sync using current date/time for incremental data synchronization.

### Message Properties & Headers

#### Modeling Basics - Access Header And Properties In XPATH And Conditions
Shows how to access message headers and properties in XPath expressions and conditions.

#### Modeling Basics - Access Header And Properties In Message Mapping
Demonstrates accessing headers and properties within message mapping steps.

#### Modeling Basics - Access Header And Properties In XSLT Mapping
Shows how to access headers and properties in XSLT transformations.

#### Modeling Basics - Mapping Context
Demonstrates how to use mapping context for passing data between steps.

### Decoupling Patterns

#### Modeling Basics - Decouple Flows Using JMS
Shows how to use JMS (Java Message Service) to decouple flows asynchronously.

#### Modeling Basics - Decouple Flows Using Persistence
Demonstrates using persistence to decouple flows.

#### Modeling Basics - Decouple Flows Without Persistence
Shows asynchronous decoupling without persistence using queues.

#### Modeling Basics - Decouple Flows With Polling Consumer
Demonstrates polling consumer pattern for asynchronous message retrieval.

### IDoc Integration

#### IDoc Bulk Handling - Outbound
Shows how to handle multiple IDocs in outbound scenarios from SAP systems.

#### IDoc Bulk Handling - Inbound
Demonstrates receiving and processing multiple IDocs into SAP systems.

## File Transfer Patterns

### File Polling

#### File Transfer - Poll File by Done File
Demonstrates polling for files using a done file marker. Ensures file is complete before processing.

#### File Transfer - Poll Folder by Fixed Done File
Shows polling a folder for files using a fixed done file naming pattern.

### File Enrichment & Merging

#### File Transfer - Concatenating Files via Poll Enrich
Shows how to concatenate multiple files using poll enrichment.

#### File Transfer - Combine XML Files via Poll Enrich
Demonstrates combining multiple XML files into a single file.

#### File Transfer - Poll And Merge Folder
Shows how to poll a folder and merge all files into a single output.

## Attachment Handling

#### Attachment Handling - Create Attachments
Demonstrates how to create message attachments programmatically.

#### Attachment Handling - Read Attachment Based On Filter Criteria
Shows how to read and process specific attachments based on filter criteria.

#### Attachment Handling - Read Multiple Attachments
Demonstrates reading and processing multiple attachments from a message.

#### Attachment Handling - Read Multiple Attachments Based On Filter Criteria
Shows how to selectively read multiple attachments based on specified criteria.

#### Attachment Handling - Replace Body With Attachment Content
Demonstrates extracting attachment content and using it as message body.

### Testing & Support

#### Generic Receiver
Integration flow mocking a receiver system for basic pattern testing.

## Core Concepts

### Message Processing
- Messages flow through steps sequentially
- Each step can transform or enrich the message
- Conditions can route messages to different paths
- Subprocesses enable modular message processing

### Data Formats
- XML: Structured, hierarchical data
- JSON: Lightweight, modern format
- CSV: Comma-separated values for tabular data
- Base64: Safe binary data transmission

### Asynchronous Processing
- Decouple sender and receiver
- Improve system reliability
- Enable scalability
- Handle peak loads better

### Persistence & Storage
- Data Store: Temporary message storage
- Persist Step: Explicit message persistence
- File Storage: Long-term data storage
- Databases: Structured data storage

## Best Practices

### Flow Design
1. **Keep flows focused** - One primary purpose per flow
2. **Use meaningful names** - Describe what the flow does
3. **Handle errors** - Plan for failure scenarios
4. **Log important events** - Enable troubleshooting
5. **Document assumptions** - Explain design decisions

### Data Handling
1. **Validate input** - Check data format and content
2. **Transform carefully** - Preserve data integrity
3. **Handle encoding** - Use appropriate character encoding
4. **Manage attachments** - Clean up after processing
5. **Persist strategically** - Store what you need

### Performance
1. **Minimize transformations** - Reduce processing overhead
2. **Use efficient formats** - Choose appropriate data formats
3. **Batch when possible** - Process multiple items together
4. **Monitor performance** - Track execution times
5. **Optimize queries** - Reduce data retrieval time

### Integration
1. **Understand source systems** - Know data formats and protocols
2. **Understand target systems** - Know requirements and limitations
3. **Test thoroughly** - Verify with real data
4. **Plan for growth** - Design for scalability
5. **Document interfaces** - Maintain current documentation

## Common Integration Scenarios

### Real-Time Order Processing
1. Receive order via HTTP
2. Validate order data
3. Transform to ERP format
4. Send to SAP ERP
5. Return confirmation to caller

### Batch File Processing
1. Poll folder for files
2. Validate file format
3. Transform to target format
4. Send to destination
5. Archive processed file

### Data Synchronization
1. Query source system
2. Compare with target
3. Identify changes (delta)
4. Transform changes
5. Apply to target system

### Message Enrichment
1. Receive initial message
2. Look up additional data
3. Combine with original message
4. Transform combined data
5. Send to destination

## Related Documentation

For comprehensive guidance on integration basics, refer to:
- SAP Help Portal: Integration Flow Modeling Guide
- Data format transformation documentation
- File handling and attachment guides
- IDoc integration documentation

---
Co-authored by [Nova](https://www.compassap.ai/portfolio/nova.html)
