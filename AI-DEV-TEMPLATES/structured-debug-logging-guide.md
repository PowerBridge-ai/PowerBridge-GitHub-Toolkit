# Structured Debug Logging Guide

## Overview

This guide details our approach to implementing a structured, emoji-based logging system for effective debugging across client and server components. The system provides clear visual indicators, consistent formatting, and multiple verbosity levels to aid in rapid troubleshooting and development.

## Table of Contents

1. [Introduction](#introduction)
2. [Core Principles](#core-principles)
3. [Standard Log Types and Emoji Patterns](#standard-log-types-and-emoji-patterns)
4. [Implementation Patterns](#implementation-patterns)
   - [Client-Side Logger](#client-side-logger)
   - [Server-Side Logger](#server-side-logger)
5. [Verbosity Levels](#verbosity-levels)
6. [Log Context Enrichment](#log-context-enrichment)
7. [Troubleshooting Workflow](#troubleshooting-workflow)
8. [Common Log Patterns](#common-log-patterns)
9. [Best Practices](#best-practices)
10. [Advanced Techniques](#advanced-techniques)
11. [Conclusion](#conclusion)

## Introduction

Effective debugging requires visibility into application behavior. Our structured logging approach creates a consistent, easily scannable log output that communicates the following information at a glance:

- Which component is logging
- What type of operation is occurring
- Whether it succeeded or failed
- Relevant contextual data
- The sequence of operations

By using emoji prefixes, standardized formatting, and consistent patterns, we create logs that are both human-readable and easy to filter programmatically.

## Core Principles

Our structured logging system is built around five core principles:

1. **Visual Category Identification**: Use emojis as prefixes to instantly identify log categories
2. **Component Source Tagging**: Always identify the source component/module in square brackets
3. **Progressive Verbosity**: Implement multiple levels of detail for different debugging needs
4. **Consistent Formatting**: Follow established patterns to make logs easily scannable
5. **Action-Result Pairing**: Log both the initiation and completion of important operations

## Standard Log Types and Emoji Patterns

| Category | Emoji | Description | Usage |
|----------|-------|-------------|-------|
| Debug | 🔍 | Detailed troubleshooting | Internal operations, variable inspection |
| Info | ℹ️ | General information | System status, component initialization |
| Success | ✅ | Successful operations | API responses, process completion |
| Warning | ⚠️ | Potential issues | Edge cases, deprecated features |
| Error | ❌ | Failures | API errors, validation failures |
| Security | 🔐 | Authentication/Authorization | Token validation, permissions |
| API | 📡 | API Communication | Requests, responses, endpoints |
| Data | 📦 | Data operations | Reading, writing, transforming |
| Initialization | 🏗️ | Component lifecycle | Setup, initialization, mounting |
| Performance | ⏱️ | Timing and performance | Durations, benchmarks |
| Admin | 👑 | Admin-related operations | Privilege checks, admin actions |
| Token | 🔑 | Token management | JWT, authentication tokens |
| User | 👤 | User operations | User data, profiles |
| Config | ⚙️ | Configuration | Environment, settings |
| Navigation | 🧭 | Routing/Navigation | Route changes |
| Storage | 🗃️ | Storage operations | localStorage, sessionStorage |
| Database | 🛢️ | Database operations | Queries, transactions |
| Network | 🌐 | Network operations | Fetch, WebSockets |
| Animation | 🎞️ | UI animations | Transitions, effects |
| Event | 📢 | Event handling | User interactions, system events |

## Implementation Patterns

### Client-Side Logger

#### Basic Logger Utility

```typescript
// src/utils/logger.ts
export const createLogger = (component: string) => {
  return {
    debug: (emoji: string, msg: string) => console.log(`🔍 [${component}] ${emoji} ${msg}`),
    info: (emoji: string, msg: string) => console.log(`ℹ️ [${component}] ${emoji} ${msg}`),
    success: (emoji: string, msg: string) => console.log(`✅ [${component}] ${emoji} ${msg}`),
    error: (emoji: string, msg: string) => console.error(`❌ [${component}] ${emoji} ${msg}`),
    warn: (emoji: string, msg: string) => console.warn(`⚠️ [${component}] ${emoji} ${msg}`),
    security: (emoji: string, msg: string) => console.log(`🔐 [${component}] ${emoji} ${msg}`),
    api: (emoji: string, msg: string) => console.log(`📡 [${component}] ${emoji} ${msg}`),
    perf: (emoji: string, msg: string) => console.log(`⏱️ [${component}] ${emoji} ${msg}`),
    // Log with multiple verbosity levels
    v1: (emoji: string, msg: string) => console.log(`[${component}] ${emoji} ${msg}`),
    v2: (emoji: string, msg: string) => {
      if (localStorage.getItem('logLevel') && parseInt(localStorage.getItem('logLevel')!) >= 2) {
        console.log(`[${component}] ${emoji} ${msg}`);
      }
    },
    v3: (emoji: string, msg: string) => {
      if (localStorage.getItem('logLevel') && parseInt(localStorage.getItem('logLevel')!) >= 3) {
        console.log(`[${component}] ${emoji} ${msg}`);
      }
    }
  };
};
```

#### Component Usage Example

```typescript
// In your component
import { createLogger } from 'src/utils/logger';

// Create component-specific logger
const logger = createLogger('SESSION_LOGS');

const SessionLogs = () => {
  const fetchSessionLogs = async () => {
    logger.info('🔄', 'Fetching session logs');
    logger.debug('📡', 'Making API request to /api/csv/admin_files/session_log.csv');
    
    try {
      const response = await api.get('/api/csv/admin_files/session_log.csv');
      logger.success('✅', `API request successful: ${response.status}`);
      logger.success('✅', `Received ${response.data.length} session logs`);
      logger.debug('🔍', `First log item structure: ${JSON.stringify(response.data[0])}`);
      
      // Process data
      setLogs(response.data);
    } catch (error: any) {
      logger.error('❌', `API request failed: ${error.message}`);
      if (error.response) {
        logger.error('🔍', `Error status: ${error.response.status}`);
        logger.error('🔍', `Error details: ${JSON.stringify(error.response.data)}`);
      }
      setNotification({
        open: true,
        message: 'Error fetching session logs',
        severity: 'error'
      });
    } finally {
      setLoading(false);
    }
  };
  
  // Rest of component
};
```

#### Enhanced Logger with Timing

```typescript
// Advanced logger with timing capabilities
export const createAdvancedLogger = (component: string) => {
  const timers = new Map<string, number>();
  const logger = createLogger(component);
  
  return {
    ...logger,
    // Start timing an operation
    startTimer: (operationName: string) => {
      timers.set(operationName, performance.now());
      logger.perf('⏱️', `Started timing: ${operationName}`);
    },
    // End timing and log the duration
    endTimer: (operationName: string) => {
      const startTime = timers.get(operationName);
      if (startTime) {
        const duration = performance.now() - startTime;
        logger.perf('⏱️', `${operationName} completed in ${duration.toFixed(2)}ms`);
        timers.delete(operationName);
        return duration;
      } else {
        logger.warn('⚠️', `No timer found for operation: ${operationName}`);
        return null;
      }
    },
    // Log with data inspection (truncated for large objects)
    inspectData: (emoji: string, label: string, data: any) => {
      const stringify = (obj: any) => {
        try {
          const str = JSON.stringify(obj, null, 2);
          if (str.length > 500) {
            return str.substring(0, 500) + '... [truncated]';
          }
          return str;
        } catch (e) {
          return '[Cannot stringify object]';
        }
      };
      
      logger.debug(emoji, `${label}: ${stringify(data)}`);
    }
  };
};
```

### Server-Side Logger

```typescript
// Server-side logger (Node.js)
const colors = {
  reset: '\x1b[0m',
  red: '\x1b[31m',
  green: '\x1b[32m',
  yellow: '\x1b[33m',
  blue: '\x1b[34m',
  magenta: '\x1b[35m',
  cyan: '\x1b[36m',
};

export const logger = {
  debug: (emoji, message) => {
    if (process.env.LOG_LEVEL >= '3') {
      console.log(`${colors.blue}[0] [1] 🔍 [DEBUG] ${emoji} ${message}${colors.reset}`);
    }
  },
  info: (emoji, message) => {
    if (process.env.LOG_LEVEL >= '2') {
      console.log(`${colors.cyan}[0] [1] ℹ️ [INFO] ${emoji} ${message}${colors.reset}`);
    }
  },
  success: (emoji, message) => {
    if (process.env.LOG_LEVEL >= '1') {
      console.log(`${colors.green}[0] [1] ✅ [SUCCESS] ${emoji} ${message}${colors.reset}`);
    }
  },
  error: (emoji, message) => {
    console.error(`${colors.red}[0] [1] ❌ [ERROR] ${emoji} ${message}${colors.reset}`);
  },
  warn: (emoji, message) => {
    console.warn(`${colors.yellow}[0] [1] ⚠️ [WARNING] ${emoji} ${message}${colors.reset}`);
  },
  security: (emoji, message) => {
    console.log(`${colors.magenta}[0] [1] 🔐 [SECURITY] ${emoji} ${message}${colors.reset}`);
  },
  api: (emoji, message) => {
    console.log(`${colors.cyan}[0] [1] 📡 [API] ${emoji} ${message}${colors.reset}`);
  },
  // Advanced server-side logging with request context
  request: (req, emoji, message) => {
    const reqId = req.id || 'unknown';
    const method = req.method || 'UNKNOWN';
    const path = req.path || req.url || 'unknown';
    const userAgent = req.headers['user-agent'] || 'unknown';
    
    console.log(`${colors.cyan}[0] [1] 📡 [API:${reqId}] [${method} ${path}] ${emoji} ${message}${colors.reset}`);
  }
};
```

#### Server Middleware for Request Logging

```typescript
// Express middleware for request logging
export const requestLogger = (req, res, next) => {
  // Generate unique request ID
  req.id = Math.random().toString(36).substring(2, 15);
  
  const start = Date.now();
  logger.request(req, '🔄', 'Request started');
  
  // Log request body for POST/PUT requests
  if ((req.method === 'POST' || req.method === 'PUT') && req.body) {
    logger.debug('📦', `Request body: ${JSON.stringify(req.body, null, 2)}`);
  }
  
  // Log query parameters if present
  if (Object.keys(req.query).length > 0) {
    logger.debug('🔍', `Query params: ${JSON.stringify(req.query, null, 2)}`);
  }
  
  // Override response methods to log responses
  const originalSend = res.send;
  res.send = function(body) {
    const duration = Date.now() - start;
    logger.request(req, '✅', `Response sent (${res.statusCode}) in ${duration}ms`);
    return originalSend.call(this, body);
  };
  
  next();
};
```

## Verbosity Levels

Implement different verbosity levels to control the amount of logging:

1. **Level 1 (Error/Critical)**: Only show errors and critical warnings
2. **Level 2 (Standard)**: Show errors, warnings, and important operations
3. **Level 3 (Verbose)**: Show all details including debug information
4. **Level 4 (Trace)**: Show extremely detailed step-by-step tracing

### Setting Verbosity Level

```typescript
// Browser console
localStorage.setItem('logLevel', '3'); // Set to verbose mode

// Server environment
// In .env file
LOG_LEVEL=3
```

### Conditional Logging Based on Environment

```typescript
// src/utils/logger.ts
export const createEnvironmentLogger = (component: string) => {
  const logger = createLogger(component);
  
  // Only log in development environment
  if (process.env.NODE_ENV !== 'development') {
    // Replace all methods with no-op functions in production
    return Object.keys(logger).reduce((acc, key) => {
      acc[key] = () => {}; // No-op function
      return acc;
    }, {} as typeof logger);
  }
  
  return logger;
};
```

## Log Context Enrichment

Enhance logs with relevant context to make debugging more effective:

### User Context

```typescript
// Add user context to logs
export const createUserContextLogger = (component: string, userContext) => {
  const logger = createLogger(component);
  
  return {
    ...logger,
    userAction: (emoji: string, action: string, userId: string) => {
      logger.info(emoji, `User ${userId} performed: ${action}`);
    },
    withUser: (userId: string) => {
      return {
        ...logger,
        debug: (emoji: string, msg: string) => 
          logger.debug(emoji, `[User:${userId}] ${msg}`),
        info: (emoji: string, msg: string) => 
          logger.info(emoji, `[User:${userId}] ${msg}`),
        // ... other methods with user context
      };
    }
  };
};
```

### Session Context

```typescript
// Add session context to logs
export const createSessionLogger = (component: string, sessionId: string) => {
  const logger = createLogger(component);
  
  return Object.keys(logger).reduce((acc, key) => {
    acc[key] = (emoji: string, msg: string) => 
      logger[key](emoji, `[Session:${sessionId.substring(0, 8)}] ${msg}`);
    return acc;
  }, {} as typeof logger);
};
```

## Troubleshooting Workflow

Follow this systematic approach to debug issues using the structured logs:

1. **Initialize with Standard Level**
   - Start with `logLevel=2` to see important operations without noise

2. **Identify Problem Area**
   - Look for error messages (❌) or warnings (⚠️)
   - Note which component is reporting issues `[COMPONENT_NAME]`

3. **Increase Verbosity**
   - Set `logLevel=3` to see detailed debug information
   - Focus on logs from the problematic component

4. **Trace Operation Flow**
   - Follow the sequence of operations:
     - 🔄 Operation started
     - 📡 API calls
     - 📦 Data handling
     - ✅/❌ Operation result

5. **Examine Related Components**
   - Check logs from related components (Auth, API client)
   - Look for security or permission issues (🔐, 👑)

6. **Inspect Data Structures**
   - Examine 🔍 debug logs with data structures
   - Look for unexpected data formats or missing fields

7. **Verify Authentication**
   - Check 🔑 token-related logs
   - Verify if the user has proper permissions

8. **Check API Communication**
   - Review 📡 API request logs
   - Verify headers, parameters, and response codes

9. **Analyze Performance Issues**
   - Look for ⏱️ performance logs
   - Check for slow operations or timeouts

## Common Log Patterns

### Authentication Flow

```
[AUTH_CONTEXT] 🏗️ AuthProvider component initialized
[AUTH_CONTEXT] 🔄 AuthProvider useEffect triggered
[AUTH_CONTEXT] 🔍 Checking for existing user session
[AUTH_CONTEXT] 🗃️ All localStorage keys: ["token"]
[AUTH_CONTEXT] 🎟️ Found token in localStorage: eyJhbGciOiJIUzI...
[AUTH_CONTEXT] 🔐 Verifying token with server
[API] 📤 GET request to /api/auth/me
[API] 🔑 Token found in localStorage
[API] 📤 Added Authorization header: Bearer eyJhbGciOiJIUzI...
[API] 📥 Response from /api/auth/me: 200
[AUTH_CONTEXT] ✅ Session verified for user: user@example.com
```

### API Request Cycle

```
[COMPONENT] 🔄 Fetching data
[COMPONENT] 📡 Making API request to /api/endpoint
[API] 📤 GET request to /api/endpoint
[API] 🔑 Token found in localStorage
[API] 📤 Added Authorization header: Bearer eyJhbGciOiJIUzI...
[API] 📥 Response from /api/endpoint: 200
[COMPONENT] ✅ API request successful: 200
[COMPONENT] 📦 Processing response data
```

### Error Scenarios

```
[COMPONENT] 🔄 Fetching data
[COMPONENT] 📡 Making API request to /api/endpoint
[API] 📤 GET request to /api/endpoint
[API] ❌ No auth token found in localStorage
[API] 📥 Response from /api/endpoint: 401
[COMPONENT] ❌ API request failed: Request failed with status code 401
[COMPONENT] 🔍 Error status: 401
[COMPONENT] 🔍 Error details: {"message":"Unauthorized"}
```

### Data Processing Flow

```
[COMPONENT] 📦 Starting data processing
[COMPONENT] 🔍 Input data: [array with 10 items]
[COMPONENT] 🔍 Processing item 1/10
[COMPONENT] 🔍 Transforming format for item 1
[COMPONENT] ✅ Item 1 processed successfully
[COMPONENT] 🔍 Processing item 2/10
[COMPONENT] ⚠️ Item 2 has missing fields, using fallbacks
[COMPONENT] ✅ Item 2 processed with fallbacks
[COMPONENT] ✅ Data processing complete - 10 items processed
```

## Best Practices

1. **Log Action Pairs**: Always log both the start and completion of important operations
2. **Include Relevant Context**: Add IDs, counts, and status codes to provide context
3. **Be Consistent**: Use the same emoji and format for similar operations across components
4. **Avoid Sensitive Data**: Never log full tokens, passwords, or personal information
5. **Use Structured Data**: For complex objects, use JSON.stringify with selective fields
6. **Group Related Logs**: Use the same emoji prefix for related operations
7. **Add Timing Information**: For performance-sensitive operations, include timing data
8. **Use Distinctive Patterns**: Create unique log patterns that are easily searchable
9. **Keep Logs Clean**: Delete or disable debugging logs in production builds
10. **Use Log Levels**: Leverage verbosity levels to control log output
11. **Log Failures Thoroughly**: Include all relevant details for error conditions
12. **Avoid Log Spam**: Don't log inside tight loops or high-frequency operations

## Advanced Techniques

### Log Filtering in DevTools

Use DevTools console filtering to focus on specific log types:

```javascript
// Filter by component
console.filter('[AUTH_CONTEXT]')

// Filter by emoji
console.filter('🔍')

// Filter by log level
console.filter('❌ [ERROR]')

// Combine filters
console.filter('[API]').filter('✅')
```

### Custom Console Styling

Add custom styling to make logs more readable:

```typescript
const logWithStyle = (message: string, style: string) => {
  console.log(`%c${message}`, style);
};

// Usage
logWithStyle('🔍 [DEBUG] Starting operation', 'color: blue; font-weight: bold;');
```

### Log Transport

For production environments, implement log transport to centralized logging systems:

```typescript
// Example with remote logging service
const remoteLogger = (level: string, component: string, emoji: string, message: string) => {
  // Send log to remote service
  fetch('/api/logs', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      level,
      component,
      emoji,
      message,
      timestamp: new Date().toISOString(),
      userAgent: navigator.userAgent,
      url: window.location.href
    })
  }).catch(err => {
    // Silent catch to avoid infinite logging loop
    console.error('Failed to send log to remote service', err);
  });
};
```

### Log Grouping

Group related logs for better readability:

```typescript
// Group related logs
export const groupLogs = (component: string, operation: string, fn: () => void) => {
  console.group(`[${component}] 🔄 ${operation}`);
  try {
    fn();
  } finally {
    console.groupEnd();
  }
};

// Usage
groupLogs('USER_COMPONENT', 'User profile update', () => {
  logger.debug('🔍', 'Starting profile update');
  logger.debug('📦', 'Processing user data');
  logger.success('✅', 'Profile updated successfully');
});
```

## Example Structured Debug Sequence

Here's a complete sequence showing how to debug an authentication and data loading process:

```typescript
// 1. Set high verbosity for debugging
localStorage.setItem('logLevel', '3');

// 2. Observe the auth flow
// Look for these log patterns:
// [AUTH_CONTEXT] 🏗️ AuthProvider component initialized
// [AUTH_CONTEXT] 🔑 Found token in localStorage...
// [AUTH_CONTEXT] 👑 Token has admin privileges...

// 3. Check component initialization
// [COMPONENT] 🔄 Fetching data...
// [COMPONENT] 👤 Current user from context...

// 4. Verify API communication
// [API] 📤 GET request to...
// [API] 🔑 Token found in localStorage...
// [API] 📥 Response from...

// 5. Examine data processing
// [COMPONENT] 📦 Response data...
// [COMPONENT] 🔍 First item structure...
```

## Conclusion

Implementing a structured emoji-based logging system provides clear visual indicators that make debugging more efficient and intuitive. By following consistent patterns and leveraging verbosity levels, developers can quickly identify issues and understand application behavior.

Key benefits of this approach include:
- Faster troubleshooting with visual scanning
- Consistent logging patterns across components
- Enhanced context for debugging complex issues
- Flexible verbosity for different debugging needs
- Clear sequence visualization for operation flows

Adopt these logging patterns across your application to create a more maintainable, debuggable codebase that accelerates development and issue resolution. 