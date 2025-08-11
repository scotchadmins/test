# 📋 uploadVisitingcardImage API Documentation

## 🔧 **Operation Details**

| Property | Value |
|----------|-------|
| **Operation Name** | `uploadVisitingcardImage` |
| **HTTP Method** | `POST` |
| **Endpoint** | `/webservice.php` |
| **Authentication** | Required (session-based) |
| **Content Type** | `application/x-www-form-urlencoded` |

---

## 📝 **API Call Format**

```http
POST /webservice.php
Content-Type: application/x-www-form-urlencoded

operation=uploadVisitingcardImage&sessionName=YOUR_SESSION_ID&recordId=75x4604387&fieldName=visitingcard_tks_dp_image&imageData=BASE64_IMAGE_DATA&fileName=image.png
```

---

## 🔑 **Required Parameters**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `operation` | string | ✅ | Operation name | `uploadVisitingcardImage` |
| `sessionName` | string | ✅ | Valid Vtiger session ID | `198820176899d7669c7bf` |
| `recordId` | string | ✅ | Visitingcard record ID | `75x4604387` |
| `fieldName` | string | ✅ | Image field name | `visitingcard_tks_dp_image` |
| `imageData` | string | ✅ | Base64 encoded image | `iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8/5+hHgAHggJ/PchI7wAAAABJRU5ErkJggg==` |
| `fileName` | string | ✅ | Original filename | `profile_photo.png` |

**💡 Quick Tip:** To convert an image file to base64, use: `$imageData = base64_encode(file_get_contents($imagePath));`

---

## 🖼️ **Available Image Fields**

### **1. Profile/DP Image**
```json
{
  "fieldName": "visitingcard_tks_dp_image",
  "description": "Profile/Display Picture Image",
  "purpose": "User profile photo or display picture",
  "recommended_size": "200x200px to 500x500px",
  "supported_formats": ["JPG", "PNG", "GIF", "WEBP"]
}
```

### **2. Company Logo**
```json
{
  "fieldName": "visitingcard_tks_company_logo",
  "description": "Company/Organization Logo",
  "purpose": "Business branding and identification",
  "recommended_size": "300x100px to 600x200px",
  "supported_formats": ["JPG", "PNG", "GIF", "WEBP"]
}
```

### **3. QR Code**
```json
{
  "fieldName": "visitingcard_tks_qr_code",
  "description": "QR Code Image",
  "purpose": "Quick access to contact information",
  "recommended_size": "200x200px to 400x400px",
  "supported_formats": ["PNG", "JPG"]
}
```

---

## 📱 **CURL Examples**

### **1. Upload Profile Image**
```bash
curl -X POST http://localhost/fancymobilenumber/webservice.php \
  -d 'operation=uploadVisitingcardImage' \
  -d 'sessionName=198820176899d7669c7bf' \
  -d 'recordId=75x4604387' \
  -d 'fieldName=visitingcard_tks_dp_image' \
  -d 'imageData=iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8/5+hHgAHggJ/PchI7wAAAABJRU5ErkJggg==' \
  -d 'fileName=profile_photo.png'
```

### **2. Upload Company Logo**
```bash
curl -X POST http://localhost/fancymobilenumber/webservice.php \
  -d 'operation=uploadVisitingcardImage' \
  -d 'sessionName=198820176899d7669c7bf' \
  -d 'recordId=75x4604387' \
  -d 'fieldName=visitingcard_tks_company_logo' \
  -d 'imageData=BASE64_ENCODED_LOGO_DATA' \
  -d 'fileName=company_logo.png'
```

### **3. Upload QR Code**
```bash
curl -X POST http://localhost/fancymobilenumber/webservice.php \
  -d 'operation=uploadVisitingcardImage' \
  -d 'sessionName=198820176899d7669c7bf' \
  -d 'recordId=75x4604387' \
  -d 'fieldName=visitingcard_tks_qr_code' \
  -d 'imageData=BASE64_ENCODED_QR_DATA' \
  -d 'fileName=qr_code.png'
```

---

## 🔐 **Authentication (Login First)**

### **Step 1: Login to Get Session**
```bash
curl -X POST http://localhost/fancymobilenumber/webservice.php \
  -d 'operation=login' \
  -d 'username=admin' \
  -d 'accessKey=YOUR_ACCESS_KEY'
```

### **Step 2: Use Session ID**
```json
{
  "success": true,
  "result": {
    "sessionName": "198820176899d7669c7bf",
    "userId": "1",
    "user_name": "admin"
  }
}
```

---

## 📊 **Response Format**

### **Success Response**
```json
{
  "success": true,
  "result": {
    "success": true,
    "record_id": "75x4604387",
    "field_name": "visitingcard_tks_dp_image",
    "filename": "4604387_dp_image.png",
    "file_path": "storage/2024/December/week4/4604387_dp_image.png",
    "attachment_id": "12345",
    "image_info": {
      "width": 200,
      "height": 200,
      "type": 2,
      "mime": "image/png",
      "size": 12345
    },
    "cleanup_stats": {
      "existing_deleted": 1,
      "non_standard_removed": 0
    },
    "message": "Image uploaded successfully with field-specific cleanup and standardized naming"
  }
}
```

### **Error Response**
```json
{
  "success": false,
  "error": {
    "code": "INVALID_ID",
    "message": "Invalid record ID format"
  }
}
```

---

## ⚠️ **Important Notes**

### **Field-Specific Behavior**
- **Each field is independent** - uploading to one field doesn't affect others
- **Old images are automatically deleted** when uploading to the same field
- **Filenames are standardized** as `{recordId}_{fieldPrefix}.{extension}`

### **Image Requirements**
- **Format**: JPG, PNG, GIF, WEBP supported
- **Size**: Recommended max 2MB per image
- **Dimensions**: Varies by field type (see field details above)
- **Encoding**: Must be valid base64

### **Database Updates**
- **Attachment records** are created in `vtiger_attachments`
- **Field values** are updated in `vtiger_visitingcard`
- **Relationships** are maintained in `vtiger_seattachmentsrel`

---

## 🧪 **Testing Examples**

### **1. Test with 1x1 Pixel PNG**
```bash
# Base64 for 1x1 transparent PNG
imageData="iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8/5+hHgAHggJ/PchI7wAAAABJRU5ErkJggg=="
```

### **2. Test with Sample JPG**
```bash
# Convert your image to base64
base64 -i your_image.jpg | tr -d '\n'
```

### **3. Verify Upload**
```bash
# Check if file exists
ls -la storage/2024/December/week4/4604387_dp_image.png

# Check database
mysql -u root -p -e "SELECT * FROM vtiger_visitingcard WHERE visitingcardid = 4604387;"
```

---

## 🔍 **Troubleshooting**

### **Common Errors**

| Error Code | Description | Solution |
|------------|-------------|----------|
| `INVALID_ID` | Record ID format incorrect | Use format: `75x4604387` |
| `RECORDNOTFOUND` | Visitingcard doesn't exist | Verify record ID in database |
| `INVALID_PARAMETER` | Missing required parameter | Check all required fields |
| `INVALID_PARAMETER` | Invalid image data | Ensure valid base64 encoding |
| `INTERNAL_ERROR` | Database/file system error | Check permissions and storage |

### **Debug Steps**
1. **Verify session is valid** - Check login response
2. **Confirm record exists** - Query `vtiger_visitingcard` table
3. **Check image format** - Validate base64 and image content
4. **Verify storage permissions** - Ensure write access to storage directory
5. **Check database connection** - Confirm Vtiger database is accessible

---

## 📚 **Related Documentation**

- [Vtiger Webservice API Reference](https://wiki.vtiger.com/vtiger6/index.php/Webservice_API_Reference)
- [Image Upload Best Practices](https://wiki.vtiger.com/vtiger6/index.php/File_Upload)
- [Session Management](https://wiki.vtiger.com/vtiger6/index.php/Session_Management)

---

## 📞 **Support**

For technical support or questions about this API:
- **Email**: support@yourcompany.com
- **Documentation**: https://docs.yourcompany.com
- **API Status**: https://status.yourcompany.com

---

*Last Updated: December 2024*  
*Version: 1.0*  
*API Version: vtiger_6.x*
