# Environment Variable Handling in MCP (Model Context Protocol)

This document provides a comprehensive overview of how environment variables are handled in MCP servers and applications.

## Core Concepts

### Default Environment Variables

MCP servers automatically inherit a limited subset of system environment variables:
- `USER`: Current user name
- `HOME`: User's home directory
- `PATH`: System path

This restriction helps maintain security and predictability across different deployment environments.

## Configuration

### Setting Custom Environment Variables

Custom environment variables can be configured in the `claude_desktop_config.json` file:

```json
{
  "mcpServers": {
    "myserver": {
      "command": "mcp-server-myapp",
      "env": {
        "MYAPP_API_KEY": "your_api_key_here",
        "MYAPP_DEBUG": "true",
        "CUSTOM_VAR": "custom_value"
      }
    }
  }
}
```

### Loading Environment Variables in Code

#### TypeScript/Node.js Example
```typescript
import dotenv from 'dotenv';

dotenv.config();

const API_KEY = process.env.MYAPP_API_KEY;
if (!API_KEY) {
  throw new Error('MYAPP_API_KEY environment variable is required');
}
```

#### Python Example
```python
import os
from dotenv import load_dotenv

load_dotenv()

API_KEY = os.getenv('MYAPP_API_KEY')
if not API_KEY:
    raise ValueError('MYAPP_API_KEY environment variable is required')
```

## Best Practices

### Security
1. **Sensitive Data**
   - Store API keys, secrets, and credentials as environment variables
   - Never commit environment variables to version control
   - Use `.env` files for local development only

2. **Validation**
   - Validate required environment variables during server initialization
   - Provide clear error messages when required variables are missing
   - Document all required environment variables

### Development Workflow
1. **Local Development**
   - Use `.env` files for local development
   - Include a `.env.example` file in version control
   - Document environment variable requirements in README

2. **Deployment**
   - Set environment variables through proper deployment configuration
   - Use different values for development, staging, and production
   - Implement proper secret management in production

## Common Issues and Solutions

### Initialization Problems
1. **Missing Variables**
   - Symptom: Server fails to start
   - Solution: Check logs for missing variable errors
   - Prevention: Document all required variables

2. **Incorrect Values**
   - Symptom: Runtime errors or unexpected behavior
   - Solution: Validate variable format and content
   - Prevention: Add validation checks during initialization

### Runtime Issues
1. **Permission Problems**
   - Symptom: Access denied errors
   - Solution: Verify environment variable permissions
   - Prevention: Document required permissions

2. **Scope Issues**
   - Symptom: Variables not available in certain contexts
   - Solution: Verify variable scope and inheritance
   - Prevention: Understand environment inheritance rules

## Debugging Environment Variables

### Logging
1. **Server-side Logging**
```python
logger.info('Environment variables loaded successfully')
logger.debug('API_KEY present: %s', bool(os.getenv('API_KEY')))
```

2. **Client-side Verification**
- Use the MCP Inspector to verify server configuration
- Check Claude Desktop logs for environment-related issues

### Troubleshooting Steps
1. Verify variable presence in configuration
2. Check server logs for initialization errors
3. Validate variable values and formats
4. Test with MCP Inspector
5. Review Claude Desktop logs