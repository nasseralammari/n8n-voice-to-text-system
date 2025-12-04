# دليل الإعداد N8N Voice-to-Text System

## متطلبات نظام التشغيل

### متطلبات البرنامج
- Node.js >= 14.0.0
- Python >= 3.8
- Docker و Docker Compose (optional)
- PostgreSQL >= 12 أو SQLite
- Redis >= 5.0 (optional للعمل مع Caching)

### متطلبات النظام
- 2GB RAM من ذاكرة الوصول
- 2GB مساحة تخزين حرة
- اتصال إنترنت

## خطوات التثبيت

### 1. استنسخ المشروع

```bash
git clone https://github.com/nasseralammari/n8n-voice-to-text-system.git
cd n8n-voice-to-text-system
```

### 2. أعدد ملف البيئة

```bash
cp .env.example .env
```

الآن عدل ملف `.env` بمقيماسك الخاصة برابطك

### 3. تثبيت الاعتماديات

```bash
npm install
pip install -r requirements.txt
```

### 4. تهيئة قاعدة البيانات

```bash
node scripts/init-db.js
```

### 5. بدء التطبيق

```bash
npm start
```

## التثبيت باستخدام Docker

### 1. بناء الصورة

```bash
docker-compose build
```

### 2. تشغيل الخدمات

```bash
docker-compose up -d
```

### 3. المطالعة

```bash
docker-compose logs -f app
```

## الوصول الأولي

بعد نجاح التاصبيب، يمكنك الوصول إلى:

- **N8N Dashboard**: http://localhost:5678
- **Application API**: http://localhost:3000
- **Health Check**: http://localhost:3000/health

## الاختبار

```bash
# الاختبارات الوحدة
npm test

# اختبارات التكامل
npm run test:integration

# تغطية الاختبارات
npm run test:coverage
```

## حل مشاكل العالمبالمين

### المنففع لا يربط بالمنفذ

إذا حصلت على حطأ EADDRINUSE، ــ المنفذ قيد الاستخدام بالفعلي:

```bash
# Linux/Mac
lsof -i :3000
kill -9 <PID>

# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

### أخطاء قاعدة البيانات

زعالبإعادة تهيئة قاعدة البيانات:

```bash
node scripts/reset-db.js
node scripts/init-db.js
```

## الخطوات الالخاصة

### إعداد Google Cloud Speech-to-Text

1. افتح علما قمته:
   - https://cloud.google.com/speech-to-text/docs/quickstart

2. نزل مفاتيح الخدمة JSON

3. ضع الملف مسار البيئة

### إعداد Azure Speech-to-Text

1. افتح علما قمته:
   - https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/

2. حمل مفاتيخ API من لوحة المفاتيح

3. عدل .env بالمفاتيح

لرصد مشاكل متقدمة، راجع وثائق TROUBLESHOOTING.md
