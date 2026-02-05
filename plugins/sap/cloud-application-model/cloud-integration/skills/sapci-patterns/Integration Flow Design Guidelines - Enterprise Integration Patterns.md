# Integration Flow Design Guidelines - Enterprise Integration Patterns

**Date:** 2026-02-05

## Overview

This guideline provides practical implementations of enterprise integration patterns for SAP Cloud Integration. It demonstrates how to solve common integration challenges using proven architectural patterns.

## Key Design Principles

- **Message routing**: Route messages based on content or conditions
- **Message transformation**: Convert between different message formats
- **Message aggregation**: Combine multiple messages into one
- **Message splitting**: Decompose complex messages
- **Quality of Service**: Ensure reliable message delivery
- **Decoupling**: Decouple sender and receiver systems

## Integration Pattern Categories

### Content-Based Routing

#### Pattern Content Based Routing - Send To Default
Routes messages to a default receiver when no specific routing condition matches.

#### Pattern Content Based Routing - Raise Error
Routes messages to error handling when no routing condition matches, providing explicit error notification.

#### Pattern Content Based Routing - Ignore If No Receiver
Silently ignores messages when no routing condition matches, useful for optional routing scenarios.

### Message Filtering & Transformation

#### Pattern Message Filter
Filters messages based on specified criteria, allowing only relevant messages to proceed.

#### Pattern Content Filter - Filter Step
Uses a dedicated filter step to process messages and remove unwanted content.

#### Pattern Content Filter - Message Mapping
Combines filtering with message mapping to transform and filter simultaneously.

### Message Splitting & Aggregation

#### Pattern Message Splitter - General Splitter
Splits a single message into multiple messages using a general-purpose splitter.

#### Pattern Message Splitter - Iterating Splitter
Splits messages by iterating through elements, useful for array-like structures.

#### Pattern Message Splitter - Message Mapping
Splits messages while applying message mapping transformations.

#### Pattern Aggregator
Combines multiple messages into a single aggregated message based on correlation criteria.

#### Pattern Scatter-Gather
Sends a message to multiple receivers and aggregates their responses.

#### Pattern Composed Message Processor
Processes multiple messages as a single logical unit.

### Recipient & Routing Patterns

#### Pattern Recipient List - Static Routing
Routes messages to a predefined list of recipients.

#### Pattern Recipient List - Dynamic Routing
Routes messages to dynamically determined recipients based on runtime conditions.

#### Pattern Recipient List - Dynamic Routing Using JMS
Uses JMS for dynamic recipient routing with asynchronous messaging.

### Content Enrichment

#### Pattern Content Enricher
Enriches incoming messages with additional data from external sources.

### Message Sequencing

#### Pattern Resequencer
Reorders messages to match a specific sequence, useful when message order is lost in asynchronous processing.

### Quality of Service Patterns

#### Pattern Quality Of Service - Scenario 01, 01a, 01b
Demonstrates basic QoS scenarios with message acknowledgment and retry logic.

#### Pattern Quality Of Service - Scenario 02, 02a, 02b
Shows intermediate QoS patterns with guaranteed delivery mechanisms.

#### Pattern Quality Of Service - Scenario 03, 03a, 03b, 03c
Covers advanced QoS scenarios with complex delivery guarantees.

#### Pattern Quality Of Service - Scenario 04, 04a, 04b
Demonstrates QoS patterns with transaction handling.

#### Pattern Quality Of Service - Scenario 05, 05a
Shows QoS patterns for idempotent message processing.

#### Pattern Quality Of Service - Scenario 06, 06a
Covers QoS patterns with error handling and recovery.

#### Pattern Quality Of Service - Scenario 07
Advanced QoS scenario with comprehensive reliability measures.

#### Pattern Quality Of Service - EOIO in General
Explains Exactly-Once-In-Order (EOIO) delivery semantics.

#### Pattern Quality Of Service - EOIO SAP RM to XI
Shows EOIO implementation for SAP RM to XI communication.

#### Pattern Quality Of Service - EOIO XI to SAP RM
Demonstrates EOIO for XI to SAP RM communication.

#### Pattern Quality Of Service - Mocked Sender
Provides a mocked sender for testing QoS patterns without real systems.

### Testing & Support

#### Generic Receiver
Integration flow mocking a receiver system for pattern testing.

## Best Practices

### Routing Patterns
1. **Use content-based routing** for intelligent message distribution
2. **Provide default handling** for unmatched routing conditions
3. **Consider performance** when using complex routing rules
4. **Log routing decisions** for troubleshooting

### Message Transformation
1. **Validate input** before transformation
2. **Handle transformation errors** gracefully
3. **Use message mapping** for complex transformations
4. **Cache mapping data** when possible

### Aggregation & Splitting
1. **Set appropriate timeouts** for aggregation
2. **Handle partial aggregations** in error scenarios
3. **Consider memory impact** when splitting large messages
4. **Correlate split messages** properly for reassembly

### Quality of Service
1. **Choose appropriate QoS level** based on business requirements
2. **Implement idempotent processing** for at-least-once delivery
3. **Use EOIO** when message order is critical
4. **Monitor delivery** and implement retry strategies

## Common Integration Scenarios

### Request-Reply with Aggregation
- Use scatter-gather to send requests to multiple services
- Aggregate responses with timeout handling
- Return combined result to caller

### Conditional Routing with Enrichment
- Route messages based on content
- Enrich with external data if needed
- Transform to target format

### Reliable Message Processing
- Implement EOIO for critical messages
- Use idempotent message processing
- Handle retries and error scenarios

### Large Message Processing
- Split large messages into chunks
- Process independently
- Aggregate results

## Related Documentation

For comprehensive guidance on enterprise integration patterns, refer to:
- SAP Help Portal: Enterprise Integration Patterns Guide
- Message routing and filtering documentation
- Quality of Service configuration guides

---
Co-authored by [Nova](https://www.compassap.ai/portfolio/nova.html)
