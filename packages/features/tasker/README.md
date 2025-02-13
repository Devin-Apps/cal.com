# Tasker Module

A robust task scheduling and execution system for Cal.com.

## Overview

The Tasker module provides:
- Asynchronous task execution
- Task scheduling and queuing
- Retry mechanisms
- Task cancellation
- Multiple implementation options

## Architecture

### Core Components

#### InternalTasker
Built-in implementation that works out of the box:
- No external dependencies
- Cron-based execution
- Built-in retry mechanism
- Task cleanup support

#### External Implementations
Pluggable external task processors:
- `TriggerDevTasker`
- `AwsSqsTasker`
- Other custom implementations

## Features

### Task Management
- Asynchronous execution
- Scheduled processing
- Automatic retries
- Task cancellation
- Queue management

### Reliability
- Retry mechanism for failed tasks
- Configurable max attempts
- Task status tracking
- Failure handling

### Flexibility
- Pluggable architecture
- Multiple implementations
- Custom task handlers
- Configurable settings

## Usage Examples

### Basic Task Creation
```typescript
const examplePayload = { example: "payload" };
await tasker.create("sendWebhook", JSON.stringify(examplePayload));
```

### Task Processing
```typescript
// /app/api/tasks/cron/route.ts
import tasker from "@calcom/features/tasker";

export async function GET() {
  await tasker.processQueue();
  return Response.json({ success: true });
}
```

### Task Cleanup
```typescript
// /app/api/tasks/cleanup/route.ts
import tasker from "@calcom/features/tasker";

export async function GET() {
  await tasker.cleanup();
  return Response.json({ success: true });
}
```

## Configuration

### Default Settings
- Cron interval: 1 minute
- Batch size: 100 tasks
- Max attempts: 3
- Task timeout: 5 minutes

### Custom Configuration
```typescript
const customTasker = new InternalTasker({
  maxAttempts: 5,
  batchSize: 50,
  timeout: 10 * 60 * 1000, // 10 minutes
});
```

## Implementation Guide

### Creating a New Tasker
1. Implement the Tasker interface
2. Add to tasker-factory.ts
3. Configure environment
4. Test implementation

### Task Handler Interface
```typescript
type TaskHandler = (payload: string) => Promise<void>;
```

## Best Practices
- Use for non-critical tasks
- Handle task failures gracefully
- Monitor task queue health
- Implement proper logging
- Regular queue maintenance

## Contributing
- Expand InternalTasker capabilities
- Create new Tasker implementations
- Improve documentation
- Add test coverage

For implementation examples, see previous contributions:
- [AWS SQS Implementation](https://github.com/calcom/cal.com/pull/12663)
- [QStash Integration](https://github.com/calcom/cal.com/pull/12658)
- [Trigger.dev Integration](https://github.com/calcom/cal.com/pull/12641)
