# vtws_getVisitingcardImages API Documentation

## Overview

The `vtws_getVisitingcardImages` webservice API retrieves visiting card image information with full URLs for the specified fields.

## API Endpoint

**Operation Name:** `getVisitingcardImages`  
**Handler Method:** `vtws_getVisitingcardImages`  
**HTTP Method:** POST  
**Authentication:** Required (Session-based)

## Purpose

This API retrieves visiting card image information including:
- Profile image (`visitingcard_tks_dp_image`)
- Company logo (`visitingcard_tks_company_logo`) 
- QR code (`visitingcard_tks_qr_code`)

The API returns full URLs for each image field, making it easy to display images in web applications.

## API Request Format

### Required Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `sessionName` | string | Valid Vtiger session ID |
| `recordId` | string | Visitingcard record ID in format `{module_id}x{record_id}` |

## API Response Format

### Success Response Structure

```json
{
    "success": true,
    "data": {
        "visitingcard_tks_dp_image": {
            "field_name": "visitingcard_tks_dp_image",
            "filename": "profile_image_123.png",
            "has_image": true,
            "full_url": "https://yourdomain.com/storage/visitingcard/profile_image_123.png",
            "attachment_info": {
                "attachment_id": "123",
                "filename": "profile_image_123.png",
                "path": "storage/visitingcard/",
                "description": "Profile Image"
            }
        },
        "visitingcard_tks_company_logo": {
            "field_name": "visitingcard_tks_company_logo",
            "filename": "company_logo_456.png",
            "has_image": true,
            "full_url": "https://yourdomain.com/storage/visitingcard/company_logo_456.png",
            "attachment_info": {
                "attachment_id": "456",
                "filename": "company_logo_456.png",
                "path": "storage/visitingcard/",
                "description": "Company Logo"
            }
        },
        "visitingcard_tks_qr_code": {
            "field_name": "visitingcard_tks_qr_code",
            "filename": null,
            "has_image": false,
            "full_url": null,
            "attachment_info": null
        },
        "record_info": {
            "record_id": "75x4604387",
            "crm_id": "4604387"
        }
    },
    "message": "Visiting card images retrieved successfully"
}
```

### Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `success` | boolean | Indicates if the API call was successful |
| `data` | object | Contains the main response data |
| `visitingcard_tks_dp_image` | object | Profile image information |
| `visitingcard_tks_company_logo` | object | Company logo information |
| `visitingcard_tks_qr_code` | object | QR code information |
| `record_info` | object | Record metadata |

### Image Field Structure

Each image field contains:

| Property | Type | Description |
|----------|------|-------------|
| `field_name` | string | The database field name |
| `filename` | string/null | Original filename or null if no image |
| `has_image` | boolean | Whether an image exists for this field |
| `full_url` | string/null | Complete URL to access the image |
| `attachment_info` | object/null | Detailed attachment metadata |

## Error Handling

### Common Error Responses

#### Invalid Record ID Format
```json
{
    "success": false,
    "error": {
        "code": "INVALIDID",
        "message": "Invalid record ID format"
    }
}
```

#### Record Not Found
```json
{
    "success": false,
    "error": {
        "code": "RECORDNOTFOUND",
        "message": "Visitingcard record not found in entity table"
    }
}
```

#### Database Query Failed
```json
{
    "success": false,
    "error": {
        "code": "INTERNAL_ERROR",
        "message": "Database query failed"
    }
}
```

## Dependencies

- Vtiger WebService Client (`Vtiger_WSClient`)
- Valid session authentication
- Database access to `vtiger_visitingcard` and related tables
- File storage access for image retrieval

## Notes

- The API automatically handles cases where images don't exist (returns null values)
- Full URLs are constructed using the `getBaseUrl()` function
- Attachment metadata includes file paths and descriptions
- Record ID must follow the format `{module_id}x{record_id}` (e.g., `75x4604387`)
