# TODO: SMS/WhatsApp Alerting Feature

This document outlines the planned implementation for SMS and WhatsApp alerting functionality in the LT Line Monitoring System.

## Overview

This branch (`feature/alerting-sms-whatsapp`) focuses on adding real-time notification capabilities via SMS and WhatsApp to alert operators about critical events such as line trips, voltage anomalies, and system failures.

## Planned Integrations

### 1. Twilio Integration

#### SMS Alerts
- **Purpose**: Send SMS notifications to operators when critical events occur
- **Implementation**:
  - Integrate Twilio API for SMS messaging
  - Configure phone number verification and authentication
  - Set up message templates for different alert types
  - Implement rate limiting to prevent spam

#### WhatsApp Business API (via Twilio)
- **Purpose**: Send WhatsApp messages for richer notifications with formatting
- **Implementation**:
  - Use Twilio's WhatsApp Business API
  - Create message templates with structured data
  - Include location information and severity levels
  - Add quick action buttons for acknowledgment

### 2. WhatsApp API Direct Integration

#### Official WhatsApp Business API
- **Purpose**: Direct integration for enterprise-grade messaging
- **Implementation**:
  - Set up WhatsApp Business Account
  - Configure webhooks for message delivery status
  - Implement message templates with media support
  - Add interactive buttons for operator response

### 3. Alert Trigger Mechanisms

#### Popup Triggers
- **Line Trip Events**:
  - Trigger when voltage > 180V but current < 0.05A
  - Send immediate SMS/WhatsApp alert
  - Include transformer ID, location, and timestamp

- **Voltage Anomalies**:
  - Trigger when voltage outside threshold (200V-250V)
  - Send warning notifications
  - Include current readings and trend data

- **Current Overload**:
  - Trigger when current exceeds safety threshold
  - Send critical priority alerts
  - Include power calculations and recommendations

- **System Connectivity Loss**:
  - Trigger when ESP8266 fails to report
  - Send maintenance alerts
  - Include last known status and duration

## Implementation Tasks

### Backend (Flask)
- [ ] Add Twilio SDK to requirements.txt
- [ ] Create notification service module
- [ ] Implement SMS sending functionality
- [ ] Implement WhatsApp messaging functionality
- [ ] Add configuration for phone numbers and API keys
- [ ] Create notification templates
- [ ] Add notification history tracking in database
- [ ] Implement retry logic for failed messages
- [ ] Add rate limiting and throttling
- [ ] Create admin interface for notification settings

### Frontend (Dashboard)
- [ ] Add notification settings panel
- [ ] Create UI for managing alert recipients
- [ ] Add notification history viewer
- [ ] Implement test notification feature
- [ ] Add notification acknowledgment interface
- [ ] Create notification statistics dashboard

### Database Schema
- [ ] Create `notifications` table
  - notification_id
  - transformer_id
  - alert_type
  - message_content
  - recipient_phone
  - delivery_channel (SMS/WhatsApp)
  - status (pending/sent/delivered/failed)
  - sent_timestamp
  - delivered_timestamp
- [ ] Create `notification_recipients` table
  - recipient_id
  - phone_number
  - name
  - role
  - preferred_channel
  - active_status
- [ ] Create `notification_templates` table
  - template_id
  - alert_type
  - message_template
  - channel

### Configuration
- [ ] Add Twilio credentials to .env
  - TWILIO_ACCOUNT_SID
  - TWILIO_AUTH_TOKEN
  - TWILIO_PHONE_NUMBER
  - TWILIO_WHATSAPP_NUMBER
- [ ] Add WhatsApp API credentials
- [ ] Configure alert thresholds
- [ ] Set up notification priority levels

### Testing
- [ ] Unit tests for notification service
- [ ] Integration tests with Twilio sandbox
- [ ] End-to-end testing with real phone numbers
- [ ] Load testing for multiple simultaneous alerts
- [ ] Failover testing for API unavailability

## Alert Message Templates

### Line Trip Alert
```
🚨 CRITICAL: Line Trip Detected
Transformer: {transformer_id}
Location: {location}
Voltage: {voltage}V
Current: {current}A
Time: {timestamp}
Action Required: Immediate inspection
```

### Voltage Anomaly Alert
```
⚠️ WARNING: Voltage Anomaly
Transformer: {transformer_id}
Voltage: {voltage}V (Normal: 220-240V)
Status: {status}
Time: {timestamp}
```

### System Offline Alert
```
🔧 MAINTENANCE: System Offline
Transformer: {transformer_id}
Last Seen: {last_seen}
Duration: {offline_duration}
Action: Check device connectivity
```

## Priority Levels

1. **CRITICAL**: Immediate SMS + WhatsApp to all operators
   - Line trips
   - Severe overloads
   - Safety hazards

2. **HIGH**: WhatsApp message to on-duty operators
   - Voltage anomalies
   - Current spikes
   - Multiple warnings

3. **MEDIUM**: Dashboard notification + optional SMS
   - Minor threshold breaches
   - Intermittent connectivity issues

4. **LOW**: Dashboard notification only
   - System status updates
   - Routine readings

## Future Enhancements

- [ ] Voice call alerts for critical events
- [ ] Email notifications with detailed reports
- [ ] Telegram bot integration
- [ ] SMS acknowledgment replies
- [ ] Two-way WhatsApp communication
- [ ] Automated escalation for unacknowledged alerts
- [ ] Integration with DISCOM control centers
- [ ] Geolocation-based nearest operator alerts
- [ ] Multi-language support for notifications

## Security Considerations

- Store API keys securely in environment variables
- Encrypt phone numbers in database
- Implement authentication for notification management
- Add audit logging for all sent notifications
- Validate phone numbers before sending
- Implement message content sanitization
- Set up rate limiting to prevent abuse

## Documentation Requirements

- [ ] API documentation for notification endpoints
- [ ] User guide for notification settings
- [ ] Administrator guide for Twilio/WhatsApp setup
- [ ] Troubleshooting guide for delivery issues
- [ ] Best practices for alert management

---

**Branch Status**: In Development  
**Target Completion**: TBD  
**Dependencies**: Twilio Account, WhatsApp Business Account (optional)
