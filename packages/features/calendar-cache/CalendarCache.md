# Calendar Cache

The Calendar Cache feature optimizes calendar availability checking by caching calendar data and using webhooks for real-time updates.

## Overview

Calendar Cache improves performance and reduces API calls to third-party calendar services by:
- Caching calendar availability data
- Using webhooks for real-time updates
- Automatically managing calendar subscriptions
- Cleaning up expired cache entries

## Architecture

### Components

1. **Calendar Watcher** (`/api/calendar-cache/cron`)
   - Runs every minute
   - Manages calendar subscriptions:
     - Watches new calendars (those with `googleChannelId: null`)
     - Renews expiring subscriptions (before `googleChannelExpiration`)
     - Cleans up when feature is disabled
   
2. **Cache Cleanup** (`calendar-cache-cleanup`)
   - Maintains cache health by removing expired records
   - Runs on a scheduled basis

3. **Webhook Handler** (`googlecalendar/api/webhook`)
   - Receives real-time calendar updates
   - Updates cache with fresh availability data

## Availability Checking Flow

1. Application requests availability via `CalendarService.getAvailability`
2. System checks for cached data
3. If cache exists and is valid:
   - Returns cached availability
4. If no cache exists:
   - Fetches from third-party calendar service
   - Returns fresh availability
   - Webhook will handle cache updates separately

## Configuration

### Feature Flag
Enable/disable the feature through the application settings.

### Cache Duration
Cache entries expire based on configured duration to ensure data freshness.

## Technical Considerations

### Data Types
- Note: `googleChannelExpiration` is stored as a string, which requires careful handling in date comparisons (e.g., Prisma's `lt` operator)

### Performance Impact
- Reduces API calls to third-party calendar services
- Improves response times for availability checks
- Minimizes rate limit concerns

## Troubleshooting

### Common Issues
1. Missing Calendar Updates
   - Verify webhook endpoint is accessible
   - Check calendar subscription status

2. Cache Inconsistencies
   - Confirm cleanup job is running
   - Verify webhook handler is receiving updates

### Monitoring
Monitor these metrics for system health:
- Cache hit/miss rates
- Webhook reception success rate
- Subscription renewal success rate
