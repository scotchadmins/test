# VoIP Providers Integration Documentation

## Overview

This document provides comprehensive documentation for all VoIP call providers integrated in the Be-Broker CRM system. The system supports multiple VoIP providers simultaneously, allowing different users to configure and use different calling services based on their preferences.

## Table of Contents

1. [Integrated Providers List](#integrated-providers-list)
2. [Provider-Specific Documentation](#provider-specific-documentation)
3. [Configuration Guide](#configuration-guide)
4. [API Integration Details](#api-integration-details)
5. [Troubleshooting](#troubleshooting)
6. [Best Practices](#best-practices)

## Integrated Providers List

### 1. **OdriCom**
- **Module**: `OdriCom`
- **Type**: API-based calling
- **Features**: Agent extension management, user assignment

### 2. **SquareTalk**
- **Module**: `SquareTalk`
- **Type**: API-based calling
- **Features**: Similar to OdriCom with API integration

### 3. **BlueCloud**
- **Module**: `BlueCloud`
- **Type**: Advanced API calling with authentication
- **Features**: Username/password authentication, organization management

### 4. **CommPeak**
- **Module**: `CommPeak`
- **Type**: CRM-integrated calling
- **Features**: CRM ID integration, client management, prefix support

### 5. **ReshetCall**
- **Module**: `ReshetCall`
- **Type**: URL-based calling
- **Features**: Custom URL configuration, agent-based routing

### 6. **OmegaTelecom**
- **Module**: `OmegaTelecom`
- **Type**: Advanced API calling
- **Features**: Username/password authentication, caller ID management

### 7. **VOIPClickToCall (Generic)**
- **Module**: `VOIPClickToCall`
- **Type**: Generic VoIP configuration
- **Features**: URL-based configuration, agent number management

### 8. **PBXManager (Core Module)**
- **Module**: `PBXManager`
- **Type**: Core call management system
- **Features**: Call recording, tracking, phone lookup, gateway management

## Provider-Specific Documentation

### 1. OdriCom

#### Configuration Fields
- `agent_no`: Agent extension number
- `assigned_to`: User ID assignment

#### API Endpoint
```
https://[provider-url]/Integration/api_call.php?Mode=Call&Extension={agent_no}&Destination={number}
```

#### Implementation Details
- Uses HTTP GET request to initiate calls
- Requires agent extension and destination number
- Returns JSON response with status

#### Response Format
```json
{
    "status": "success",
    "message": "Call initiated successfully"
}
```

### 2. SquareTalk

#### Configuration Fields
- `agent_no`: Agent extension number
- `assigned_to`: User ID assignment

#### API Endpoint
```
https://[provider-url]/Integration/api_call.php?Mode=Call&Extension={agent_no}&Destination={number}
```

#### Implementation Details
Identical to OdriCom implementation with same API structure.

### 3. BlueCloud

#### Configuration Fields
- `agent_no`: Agent extension number
- `username`: Authentication username
- `password`: Authentication password
- `orgname`: Organization name
- `assigned_to`: User ID assignment

#### API Endpoint
```
https://voip.bebrokers.me/API/dialup.php?user={username}&pass={password}&org={orgname}&dial1={number}&dial2={agent_no}&Response=Yes&RFormat=json
```

#### Implementation Details
- Uses HTTP GET request with authentication parameters
- Requires username, password, organization name
- Supports both dial1 (destination) and dial2 (agent) parameters
- Returns JSON response with call status

#### Response Format
```json
{
    "data": {
        "Response": "success",
        "CallID": "12345",
        "Status": "initiated"
    }
}
```

### 4. CommPeak

#### Configuration Fields
- `crm_id`: CRM identifier
- `client_id`: Client identifier
- `agent_no`: Agent extension number
- `prefix`: Optional prefix for numbers
- `assigned_to`: User ID assignment

#### API Endpoint
```
https://click2call.pbx.commpeak.com/call/{crm_id}/{client_id}/{agent_no}/{number}[/{prefix}]
```

#### Implementation Details
- Uses HTTP GET request with path parameters
- Requires CRM ID, client ID, and agent number
- Optional prefix parameter for number formatting
- Returns JSON response with success status

#### Response Format
```json
{
    "success": true,
    "message": "OK",
    "call_id": "12345"
}
```

### 5. ReshetCall

#### Configuration Fields
- `agent_no`: Agent extension number
- `url`: Provider URL
- `assigned_to`: User ID assignment

#### API Endpoint
```
https://[provider-url]/custom/click2call?username={agent_no}&number={number}&caller_id_number={agent_no}
```

#### Implementation Details
- Uses HTTP GET request with query parameters
- Requires custom provider URL configuration
- Uses agent number as both username and caller ID
- Returns JSON response with status

#### Response Format
```json
{
    "status": "success",
    "call_id": "12345"
}
```

### 6. OmegaTelecom

#### Configuration Fields
- `agent_no`: Agent extension number
- `username`: Authentication username
- `password`: Authentication password
- `callerid`: Caller ID number
- `url`: Provider URL
- `assigned_to`: User ID assignment

#### API Endpoint
```
https://www.omega-telecom.net/api/json/calls/make/?auth_username={username}&auth_password={password}&snumber={number}&cnumber={agent_no}&callerid1={callerid}
```

#### Implementation Details
- Uses HTTP GET request with authentication
- Requires username, password, and caller ID
- Supports both source number (snumber) and calling number (cnumber)
- Returns JSON response with status

#### Response Format
```json
{
    "status": "success",
    "call_id": "12345"
}
```

### 7. VOIPClickToCall (Generic)

#### Configuration Fields
- `url`: Provider URL
- `agent_no`: Agent extension number
- `assigned_to`: User ID assignment

#### API Endpoint
```
{url}?agent={agent_no}&dest={number}
```

#### Implementation Details
- Uses HTTP GET request with simple parameters
- Adds '00' prefix to destination number
- Requires agent number and destination number
- No response validation (generic implementation)

### 8. PBXManager (Core Module)

#### Features
- Call recording and management
- Incoming/outgoing call tracking
- Phone number lookup
- Gateway configuration
- Integration with Contacts, Leads, and Accounts

#### Database Tables
- `vtiger_pbxmanager`: Main call records
- `vtiger_pbxmanager_gateway`: Gateway configurations
- `vtiger_pbxmanager_phonelookup`: Phone number mappings

## Configuration Guide

### Step 1: Access Provider Settings

1. Go to **Settings** in the CRM
2. Navigate to **Extensions** or **Other Settings**
3. Find your desired VoIP provider
4. Click on the provider name to access configuration

### Step 2: Configure Provider Settings

#### For Basic Providers (OdriCom, SquareTalk, OmegaTelecom)
1. Enter **Agent Extension Number**
2. Assign to **User**
3. Save configuration

#### For Advanced Providers (BlueCloud, CommPeak, ReshetCall)
1. Enter **Agent Extension Number**
2. Enter **Username** (if required)
3. Enter **Password** (if required)
4. Enter **Organization Name** (if required)
5. Enter **Provider URL** (if required)
6. Enter **Caller ID** (if required)
7. Assign to **User**
8. Save configuration

### Step 3: Test Configuration

1. Go to any Contact or Lead record
2. Click on the phone number
3. Verify the call is initiated through your configured provider

## API Integration Details

### Common HTTP Settings

All providers use HTTP GET requests with similar patterns:
- SSL verification is typically disabled
- Fresh connections are used for each request
- JSON responses are expected from most providers

### Error Handling

All providers implement basic error handling:
- Check response status for 'success' or 'true' values
- Validate API response format
- Handle connection timeouts gracefully

### Security Considerations

- SSL verification is disabled for all providers
- Passwords are stored in configuration
- Consider implementing encryption for sensitive data
- Use HTTPS endpoints when possible


## Troubleshooting

### Common Issues

#### 1. Call Not Initiating
**Symptoms**: Click-to-call button doesn't work
**Solutions**:
- Check provider configuration
- Verify agent extension number
- Check API endpoint URL
- Verify user assignment

#### 2. Authentication Errors
**Symptoms**: API returns authentication failure
**Solutions**:
- Verify username and password
- Check API credentials with provider
- Ensure proper URL encoding

#### 3. SSL Certificate Errors
**Symptoms**: SSL verification errors
**Solutions**:
- Check if provider URL is accessible
- Verify SSL certificate validity
- Consider updating connection settings

#### 4. Configuration Issues
**Symptoms**: Configuration not saving
**Solutions**:
- Check user permissions
- Verify module access rights
- Clear browser cache

### Debug Mode

Enable debug logging by adding debug statements to provider action files:
- Log request parameters
- Log API endpoint URLs
- Log response data
- Check error logs for troubleshooting

### Log Files

Check the following log files:
- PHP error logs
- Vtiger logs
- Web server logs
- Provider-specific logs

## Best Practices

### Security
1. **Encrypt Sensitive Data**: Store passwords and API keys encrypted
2. **Use HTTPS**: Ensure all API endpoints use HTTPS
3. **Regular Updates**: Keep provider credentials updated
4. **Access Control**: Limit provider configuration access to authorized users

### Performance
1. **Connection Pooling**: Implement connection pooling for high-volume usage
2. **Caching**: Cache provider configurations
3. **Timeout Settings**: Set appropriate connection timeouts
4. **Error Handling**: Implement comprehensive error handling

### Maintenance
1. **Regular Testing**: Test all providers regularly
2. **Backup Configurations**: Backup provider settings
3. **Monitor Usage**: Track call volumes and success rates
4. **Update Documentation**: Keep documentation current

### Integration
1. **Standardize APIs**: Use consistent API patterns across providers
2. **Fallback Mechanisms**: Implement fallback to alternative providers
3. **Monitoring**: Implement call success monitoring
4. **Reporting**: Generate usage reports for each provider

## Support and Maintenance

### Regular Maintenance Tasks
- **Weekly**: Test all provider configurations
- **Monthly**: Review call logs and success rates
- **Quarterly**: Update provider documentation
- **Annually**: Review and update security measures

### Monitoring
- Track call success rates
- Monitor API response times
- Check for provider service outages
- Review error logs regularly

### Updates
- Keep provider modules updated
- Update API endpoints as needed
- Implement new provider features
- Maintain compatibility with CRM updates

This documentation provides a comprehensive guide for managing all VoIP providers integrated in the Be-Broker CRM system. For additional support or specific provider questions, refer to the individual provider documentation or contact the system administrator.
