# Integration Flow Design Guidelines - Use the Partner Directory Appropriately

**Date:** 2026-02-05

## Overview

This guideline demonstrates how to effectively use the Partner Directory in SAP Cloud Integration for managing partner-specific configurations, dynamic routing, and runtime parameter management. The Partner Directory enables flexible, scalable integrations without hardcoding partner information.

## Key Design Principles

- **Dynamic configuration**: Store partner-specific settings in Partner Directory
- **Runtime flexibility**: Change partner settings without redeploying flows
- **Scalability**: Add new partners without flow modifications
- **Security**: Manage credentials and keys securely
- **Maintainability**: Centralize partner information management
- **Auditability**: Track partner configuration changes

## Partner Directory Patterns

### Dynamic Receiver Selection

#### Partner Directory - Dynamic Receiver
Demonstrates how to dynamically select the receiver endpoint based on Partner Directory configuration. Enables routing to different partners without flow changes.

**Dynamic Receiver Benefits:**
- Add new partners without redeployment
- Change receiver endpoints at runtime
- Support multiple receiver endpoints per partner
- Implement partner-specific routing

**Configuration Example:**
```
Partner ID: PARTNER_001
Receiver URL: https://partner1.example.com/api/endpoint
Authentication: OAuth2
```

### Dynamic Routing with Alternative Partner IDs

#### Partner Directory - Alternative Partner Id
Shows how to use alternative partner identifiers for flexible routing. Useful when partners use different ID formats.

**Alternative ID Scenarios:**
- Legacy partner IDs
- Multiple ID formats per partner
- Cross-system partner mapping
- Alias management

### Authentication & Authorization

#### Partner Directory - Authorized User
Demonstrates how to manage partner-specific user credentials and authorization in Partner Directory.

**User Management:**
- Store partner credentials securely
- Manage user access levels
- Implement role-based access
- Track user activity

### Dynamic Key Management

#### Partner Directory - AS2 Dynamic Keys
Shows how to manage AS2 (Applicability Statement 2) encryption and signing keys dynamically for each partner.

**AS2 Key Management:**
- Store public/private keys
- Manage key versions
- Implement key rotation
- Support multiple key formats

### Data Format Transformation

#### Partner Directory - XSLT Mapping
Demonstrates how to store and apply partner-specific XSLT transformations from Partner Directory.

**Partner-Specific Transformation:**
- Different message formats per partner
- Custom field mapping
- Partner-specific validation rules
- Format version management

### Testing & Support

#### Generic Receiver
Integration flow mocking a receiver system for Partner Directory testing.

## Partner Directory Capabilities

### Configuration Management

**Partner-Specific Settings:**
- Endpoint URLs
- Authentication credentials
- Encryption keys
- Message format specifications
- Transformation rules
- Routing rules
- SLA parameters

### Credential Storage

**Secure Credential Management:**
- Username/password pairs
- API keys and tokens
- SSL certificates
- Encryption keys
- SSH keys

### Dynamic Parameters

**Runtime Parameters:**
- Partner-specific timeouts
- Retry policies
- Batch sizes
- Message formats
- Routing rules

## Best Practices

### Partner Configuration

1. **Organize Partner Information**
   ```
   Partner ID Structure: COMPANY_REGION_SYSTEM
   Example: ACME_US_SAP
   
   Partner Details:
   - Company: ACME Corp
   - Region: United States
   - System: SAP ERP
   - Receiver URL: https://acme-us.example.com/api
   ```

2. **Use Meaningful Partner IDs**
   - Use company/system identifiers
   - Avoid generic names
   - Document ID scheme
   - Maintain consistency

3. **Document Partner Settings**
   - Record all partner-specific configurations
   - Document authentication methods
   - List supported message formats
   - Include SLA requirements

### Credential Management

1. **Secure Storage**
   - Never hardcode credentials
   - Use Partner Directory for storage
   - Implement access controls
   - Audit credential access

2. **Credential Rotation**
   - Implement regular rotation schedule
   - Notify partners of changes
   - Test new credentials before activation
   - Maintain backward compatibility during transition

3. **Key Management**
   - Store keys securely
   - Implement key versioning
   - Support key rotation
   - Document key formats

### Dynamic Routing

1. **Routing Configuration**
   ```
   Partner: PARTNER_001
   Primary Endpoint: https://primary.example.com
   Fallback Endpoint: https://fallback.example.com
   Timeout: 30 seconds
   Retry Count: 3
   ```

2. **Failover Strategy**
   - Define primary and fallback endpoints
   - Implement health checks
   - Use circuit breakers
   - Log routing decisions

3. **Load Balancing**
   - Distribute load across endpoints
   - Monitor endpoint performance
   - Implement round-robin routing
   - Track endpoint health

### Transformation Management

1. **Partner-Specific Transformations**
   - Store XSLT mappings
   - Version transformations
   - Document transformation rules
   - Test before deployment

2. **Format Variations**
   - Support multiple message formats
   - Implement format detection
   - Provide format conversion
   - Document format specifications

3. **Validation Rules**
   - Define partner-specific validation
   - Implement custom validators
   - Document validation rules
   - Test validation logic

## Common Scenarios

### Scenario 1: Multi-Partner Integration
**Challenge**: Support multiple partners with different requirements
**Solution**:
- Use Partner Directory for partner-specific configuration
- Implement dynamic routing based on Partner ID
- Store partner-specific transformations
- Manage partner credentials securely

**Flow Structure:**
```
1. Receive Message
2. Extract Partner ID
3. Look up Partner Configuration
4. Apply Partner-Specific Transformation
5. Route to Partner Endpoint
6. Handle Partner-Specific Errors
```

### Scenario 2: Secure Partner Communication
**Challenge**: Manage encryption keys and credentials for multiple partners
**Solution**:
- Store keys in Partner Directory
- Implement dynamic key selection
- Support key rotation
- Audit key usage

**Security Approach:**
- Encrypt keys at rest
- Implement access controls
- Log key access
- Monitor for suspicious activity

### Scenario 3: Partner Onboarding
**Challenge**: Quickly add new partners without code changes
**Solution**:
- Define partner configuration template
- Store in Partner Directory
- Implement dynamic routing
- Test with mock partner

**Onboarding Process:**
```
1. Define Partner Configuration
2. Add to Partner Directory
3. Configure Authentication
4. Test Integration
5. Deploy to Production
6. Monitor Performance
```

### Scenario 4: Partner-Specific SLAs
**Challenge**: Enforce different SLAs for different partners
**Solution**:
- Store SLA parameters in Partner Directory
- Implement dynamic timeout configuration
- Monitor SLA compliance
- Alert on SLA violations

**SLA Configuration:**
```
Partner: PARTNER_001
Response Time SLA: 5 seconds
Availability SLA: 99.9%
Max Retries: 3
Retry Interval: 1 second
```

## Partner Directory vs. Hardcoding

### Hardcoded Configuration ❌
```groovy
def receiverUrl = "https://specific-partner.example.com/api"
def username = "partner_user"
def password = "partner_password"
```

**Problems:**
- Requires redeployment for changes
- Security risk (credentials in code)
- Not scalable for multiple partners
- Difficult to audit changes

### Partner Directory Configuration ✅
```groovy
def partnerId = message.getProperty("partnerId")
def partnerConfig = lookupPartnerDirectory(partnerId)
def receiverUrl = partnerConfig.receiverUrl
def credentials = partnerConfig.credentials
```

**Benefits:**
- Runtime configuration changes
- Secure credential storage
- Scalable for multiple partners
- Easy to audit and track changes

## Integration with Flows

### Step 1: Extract Partner Identifier
```groovy
def partnerId = message.getProperty("partnerId")
// or
def partnerId = extractFromMessage(message)
```

### Step 2: Look Up Partner Configuration
```groovy
def partnerConfig = message.getProperty("partnerConfig")
// Configuration is typically looked up via dedicated step
```

### Step 3: Apply Partner-Specific Logic
```groovy
def receiverUrl = partnerConfig.receiverUrl
def transformationRule = partnerConfig.transformationRule
def credentials = partnerConfig.credentials
```

### Step 4: Execute with Partner Configuration
```groovy
// Send to partner-specific endpoint
// Apply partner-specific transformation
// Use partner-specific credentials
```

## Monitoring & Maintenance

### Monitoring

1. **Track Partner Activity**
   - Monitor message volume per partner
   - Track success/failure rates
   - Monitor response times
   - Alert on anomalies

2. **Audit Partner Access**
   - Log all configuration changes
   - Track credential usage
   - Monitor access patterns
   - Alert on suspicious activity

### Maintenance

1. **Regular Reviews**
   - Review partner configurations quarterly
   - Update deprecated settings
   - Remove inactive partners
   - Validate credentials

2. **Performance Optimization**
   - Monitor partner endpoint performance
   - Optimize routing rules
   - Update timeouts based on performance
   - Implement caching where appropriate

## Related Documentation

For comprehensive guidance on Partner Directory, refer to:
- SAP Help Portal: Partner Directory Configuration
- Dynamic routing and configuration documentation
- Credential management and security guides
- Partner onboarding best practices

---
Co-authored by [Nova](https://www.compassap.ai/portfolio/nova.html)
