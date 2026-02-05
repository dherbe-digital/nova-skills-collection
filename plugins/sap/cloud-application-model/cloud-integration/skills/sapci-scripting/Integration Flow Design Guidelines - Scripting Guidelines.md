# Integration Flow Design Guidelines - Scripting Guidelines

**Date:** 2026-02-05

## Overview

This guideline covers best practices for scripting in SAP Cloud Integration flows. It demonstrates how to use Groovy scripts effectively for data transformation, manipulation, and integration logic while maintaining security and performance.

## Key Design Principles

- **Minimal scripting**: Use built-in steps when possible
- **Security first**: Never hardcode credentials or sensitive data
- **Performance**: Write efficient scripts that don't block processing
- **Maintainability**: Write clear, documented, reusable code
- **Error handling**: Implement proper exception handling
- **Testing**: Test scripts thoroughly before deployment

## Scripting Patterns

### Data Parsing & Transformation

#### Scripting - Parse JSON
Demonstrates how to parse JSON content in a script step. Essential for working with modern APIs.

**JSON Parsing Approach:**
```groovy
// Parse JSON string
def jsonSlurper = new groovy.json.JsonSlurper()
def parsed = jsonSlurper.parseText(jsonString)

// Access properties
def value = parsed.propertyName

// Convert back to JSON
def jsonBuilder = new groovy.json.JsonBuilder()
jsonBuilder(data)
def jsonOutput = jsonBuilder.toString()
```

#### Scripting - Mapping Functions
Shows how to implement custom mapping functions in scripts. Useful for complex transformation logic.

**Function Implementation:**
- Custom transformation logic
- Reusable mapping functions
- Data validation
- Error handling

#### Scripting - Value Mapping
Demonstrates value mapping/lookup functionality in scripts. Converts values from one domain to another.

**Value Mapping Scenarios:**
- Currency conversion
- Status code translation
- System-specific value mapping
- Enumeration conversion

### Message Property & Header Access

#### Scripting - Read Url Get Parameters
Shows how to read URL query parameters from the request. Essential for parameterized integrations.

**Parameter Access:**
```groovy
// Get request parameters
def parameters = message.getHeaders()

// Access specific parameter
def paramValue = message.getProperty('parameterName')
```

#### Scripting - Read Url Path
Demonstrates extracting values from URL path segments. Useful for RESTful integrations.

**Path Extraction:**
- Extract path variables
- Parse dynamic URLs
- Route based on path

#### Scripting - MPL Custom Header
Shows how to work with custom message headers. Enables passing metadata through flows.

**Header Operations:**
- Read custom headers
- Add new headers
- Modify existing headers
- Remove headers

#### Scripting - MPL Attachment
Demonstrates working with message attachments in scripts. Enables attachment processing and manipulation.

**Attachment Operations:**
- Read attachment content
- Create attachments
- Modify attachment properties
- Remove attachments

### Security & Credentials

#### Scripting - Security Material
Shows how to securely access credentials and security materials in scripts.

**Secure Credential Access:**
```groovy
// Access credentials from secure store
def credentialName = 'MyCredential'
def username = message.getProperty('username')
def password = message.getProperty('password')

// Never hardcode credentials!
// Always use secure credential storage
```

## Scripting Best Practices

### Code Quality

1. **Keep Scripts Simple**
   - Single responsibility per script
   - Avoid complex logic in scripts
   - Use built-in steps for standard operations
   - Extract reusable logic

2. **Write Readable Code**
   - Use meaningful variable names
   - Add comments for complex logic
   - Follow consistent formatting
   - Use helper functions

3. **Error Handling**
   ```groovy
   try {
       // Script logic
       def result = processData(input)
       return result
   } catch (Exception e) {
       // Log error
       throw new RuntimeException("Processing failed: " + e.message, e)
   }
   ```

### Security

1. **Never Hardcode Credentials**
   - Use secure credential storage
   - Reference credentials by name
   - Implement credential rotation
   - Audit credential access

2. **Protect Sensitive Data**
   - Don't log passwords
   - Don't log PII
   - Encrypt sensitive data
   - Mask sensitive values in output

3. **Input Validation**
   ```groovy
   // Validate input before processing
   if (input == null || input.isEmpty()) {
       throw new IllegalArgumentException("Input cannot be empty")
   }
   ```

### Performance

1. **Optimize Script Execution**
   - Minimize external calls
   - Cache frequently accessed data
   - Use efficient algorithms
   - Avoid blocking operations

2. **Resource Management**
   - Close resources properly
   - Limit memory usage
   - Handle large data efficiently
   - Monitor script execution time

3. **Scalability**
   - Write stateless scripts
   - Avoid global variables
   - Use thread-safe operations
   - Test with large data volumes

### Maintainability

1. **Documentation**
   ```groovy
   /**
    * Transforms customer data from source to target format
    * @param sourceData The input customer data
    * @return Transformed customer data
    */
   def transformCustomer(sourceData) {
       // Implementation
   }
   ```

2. **Testing**
   - Unit test scripts independently
   - Test with real data samples
   - Test error scenarios
   - Test performance with large data

3. **Version Control**
   - Track script changes
   - Document changes
   - Review scripts before deployment
   - Maintain script history

## Common Scripting Tasks

### Task 1: JSON to XML Transformation
```groovy
import groovy.json.JsonSlurper
import groovy.xml.MarkupBuilder

def jsonInput = message.getBody(String)
def json = new JsonSlurper().parseText(jsonInput)

def writer = new StringWriter()
def xml = new MarkupBuilder(writer)

xml.root {
    json.each { key, value ->
        "${key}"(value)
    }
}

message.setBody(writer.toString())
```

### Task 2: Extract and Map Values
```groovy
def input = message.getBody(String)
def mapping = [
    'OLD_STATUS': 'NEW_STATUS',
    'PENDING': 'IN_PROGRESS',
    'DONE': 'COMPLETED'
]

def output = input
mapping.each { oldValue, newValue ->
    output = output.replace(oldValue, newValue)
}

message.setBody(output)
```

### Task 3: Add Custom Headers
```groovy
def headers = message.getHeaders()
headers.put('X-Custom-Header', 'CustomValue')
headers.put('X-Timestamp', System.currentTimeMillis().toString())

message.setHeaders(headers)
```

### Task 4: Process Attachments
```groovy
def attachments = message.getAttachments()

attachments.each { name, content ->
    // Process each attachment
    def processedContent = processAttachment(content)
    
    // Update attachment
    message.removeAttachment(name)
    message.addAttachment(name, processedContent)
}
```

## Anti-Patterns to Avoid

1. **Hardcoded Credentials**
   - ❌ Don't: `password = "myPassword123"`
   - ✅ Do: Use secure credential storage

2. **Complex Business Logic**
   - ❌ Don't: Implement complex algorithms in scripts
   - ✅ Do: Use appropriate steps or external services

3. **Synchronous External Calls**
   - ❌ Don't: Make blocking external calls
   - ✅ Do: Use asynchronous patterns

4. **Unhandled Exceptions**
   - ❌ Don't: Let exceptions propagate silently
   - ✅ Do: Implement proper error handling

5. **Global State**
   - ❌ Don't: Use global variables
   - ✅ Do: Use message properties

## Debugging Scripts

1. **Logging**
   ```groovy
   import org.slf4j.LoggerFactory
   def logger = LoggerFactory.getLogger(this.class)
   logger.info("Processing message: " + message.getBody(String))
   ```

2. **Message Inspection**
   - Log message body
   - Log headers and properties
   - Log attachments
   - Log processing state

3. **Error Diagnosis**
   - Catch and log exceptions
   - Include context in error messages
   - Log stack traces
   - Monitor script execution time

## Related Documentation

For comprehensive guidance on scripting, refer to:
- SAP Help Portal: Groovy Script Step Documentation
- Groovy language reference
- SAP Cloud Integration API documentation
- Security best practices for scripting

---
Co-authored by [Nova](https://www.compassap.ai/portfolio/nova.html)
