# وثائق تكامل API

## نظرة عامة

هذا الملف يصف جميع نقاط الطلب (Endpoints) المتاحة في نظام N8N Voice-to-Text System.

## المصادقة

يتطلب جميع الطلبات معرف Token JWT في الرأس:

```http
Authorization: Bearer <JWT_TOKEN>
```

## نقاط الطلب الأساسية

### 1. فحص الصحة

```http
GET /health
```

**الاستجابة:**
```json
{
  "status": "ok",
  "timestamp": "2024-01-01T00:00:00Z",
  "version": "1.0.0"
}
```

### 2. رفع ملف صوتي

```http
POST /api/v1/upload
Content-Type: multipart/form-data
```

**المعاملات:**
- `file` (required): الملف الصوتي
- `language` (optional): كود اللغة (default: en-US)

**الاستجابة:**
```json
{
  "success": true,
  "fileId": "file-uuid",
  "fileName": "audio.mp3",
  "size": 1024000,
  "uploadedAt": "2024-01-01T00:00:00Z"
}
```

### 3. تحويل الملف

```http
POST /api/v1/transcribe
Content-Type: application/json
```

**المعاملات:**
```json
{
  "fileId": "file-uuid",
  "language": "ar-SA",
  "model": "default"
}
```

**الاستجابة:**
```json
{
  "success": true,
  "taskId": "task-uuid",
  "status": "processing",
  "estimatedTime": 30
}
```

### 4. الحصول على حالة المهمة

```http
GET /api/v1/transcribe/{taskId}
```

**الاستجابة (معالجة):**
```json
{
  "taskId": "task-uuid",
  "status": "processing",
  "progress": 45
}
```

**الاستجابة (مكتملة):**
```json
{
  "taskId": "task-uuid",
  "status": "completed",
  "text": "النص المحول",
  "confidence": 0.95,
  "duration": 45,
  "completedAt": "2024-01-01T00:00:45Z"
}
```

### 5. قائمة الملفات

```http
GET /api/v1/files
```

**المعاملات (Query):**
- `page`: رقم الصفحة (default: 1)
- `limit`: عدد النتائج (default: 10)

**الاستجابة:**
```json
{
  "success": true,
  "files": [
    {
      "fileId": "file-uuid",
      "fileName": "audio.mp3",
      "size": 1024000,
      "uploadedAt": "2024-01-01T00:00:00Z",
      "status": "completed"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25
  }
}
```

## رموز الأخطاء

### 400 Bad Request
```json
{
  "error": "invalid_request",
  "message": "وصف الخطأ"
}
```

### 401 Unauthorized
```json
{
  "error": "unauthorized",
  "message": "توكن غير صحيح أو انتهت صلاحيته"
}
```

### 500 Internal Server Error
```json
{
  "error": "server_error",
  "message": "خطأ في الخادم"
}
```

## الأمثلة

### مثال عملي (cURL)

```bash
# 1. رفع الملف
curl -X POST http://localhost:3000/api/v1/upload \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@audio.mp3"

# 2. تحويل الملف
curl -X POST http://localhost:3000/api/v1/transcribe \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"fileId": "file-uuid"}'

# 3. فحص الحالة
curl -X GET http://localhost:3000/api/v1/transcribe/task-uuid \
  -H "Authorization: Bearer <TOKEN>"
```

## معدل الحد من الطلبات

- عام: 1000 طلب / ساعة
- رفع الملفات: 100 طلب / ساعة
- التحويل: 50 طلب / ساعة

## الإصدارات المدعومة

- `v1`: الإصدار الحالي (stable)

راجع TROUBLESHOOTING.md لمزيد من المساعدة.
