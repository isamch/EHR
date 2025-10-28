# نظام MVP بسيط للتواصل بين CareFlow والصيدلية

## نظرة عامة
نظام بسيط للتواصل بين عيادة CareFlow والصيدلية لإدارة الوصفات الطبية بدون تعقيدات API Token.

## المميزات
- ✅ بحث عن الأدوية في الصيدلية
- ✅ إرسال وصفات طبية للصيدلية
- ✅ البحث عن وصفات بالاسم في الصيدلية
- ✅ تحديث حالة الوصفة
- ✅ نظام بسيط بدون مصادقة معقدة

## الخطوات المطلوبة

### 1. إعداد الصيدلية
```bash
cd pharmacy
npm install
npm run setup  # إضافة البيانات التجريبية
npm run dev    # تشغيل الخادم على المنفذ 5001
```

### 2. إعداد العيادة (CareFlow)
```bash
cd "CareFlow EHR"
npm install
npm run dev    # تشغيل الخادم على المنفذ 5000
```

### 3. إضافة متغيرات البيئة في العيادة
أنشئ ملف `.env` في مجلد `CareFlow EHR`:
```env
PHARMACY_API_URL=http://localhost:5001/api
CLINIC_CODE=CLINIC_001
CLINIC_NAME=عيادة الرعاية الصحية
```

## كيفية الاستخدام

### 1. البحث عن الأدوية
```http
GET http://localhost:5000/api/doctor/medications/search?search=باراسيتامول
```

### 2. إرسال وصفة طبية
```http
POST http://localhost:5000/api/doctor/prescriptions/send-to-pharmacy
Content-Type: application/json

{
  "patientName": "أحمد محمد",
  "patientAge": 30,
  "patientPhone": "0123456789",
  "doctorName": "د. سارة أحمد",
  "clinicName": "عيادة الرعاية الصحية",
  "clinicCode": "CLINIC_001",
  "medications": [
    {
      "medicationName": "باراسيتامول 500mg",
      "quantity": 2,
      "dosage": "500mg",
      "duration": "7 أيام",
      "notes": "يؤخذ بعد الأكل"
    }
  ],
  "prescriptionNotes": "مريض يعاني من صداع"
}
```

### 3. البحث عن وصفة في الصيدلية
```http
GET http://localhost:5001/api/prescriptions/search/أحمد محمد
```

### 4. تحديث حالة الوصفة
```http
PUT http://localhost:5001/api/prescriptions/:id/status
Content-Type: application/json

{
  "status": "ready",
  "notes": "الوصفة جاهزة للاستلام"
}
```

## تدفق العمل الكامل

1. **الطبيب يبحث عن الأدوية** في قاعدة بيانات الصيدلية
2. **الطبيب يختار الأدوية** المطلوبة للمريض
3. **الطبيب يرسل الوصفة** للصيدلية مع بيانات المريض
4. **الصيدلية تستقبل الوصفة** وتتحقق من توفر الأدوية
5. **الصيدلية تعد الأدوية** وتحدث حالة الوصفة
6. **المريض يذهب للصيدلية** ويبحث عن اسمه
7. **الصيدلية تعطي الدواء** للمريض وتحدث الحالة

## البيانات التجريبية المضافة

### الأدوية المتاحة:
- باراسيتامول 500mg (15.50 ريال)
- أموكسيسيلين 250mg (25.00 ريال)
- إيبوبروفين 400mg (18.75 ريال)
- أوميبرازول 20mg (35.00 ريال)
- لوراتادين 10mg (22.50 ريال)

### العميل:
- اسم: عيادة الرعاية الصحية
- رمز العيادة: CLINIC_001
- شخص الاتصال: د. أحمد محمد
- الهاتف: 0123456789

## API Endpoints

### في العيادة (CareFlow) - المنفذ 5000
- `GET /api/doctor/medications/search` - البحث عن الأدوية
- `GET /api/doctor/medications/available` - الأدوية المتاحة
- `POST /api/doctor/prescriptions/send-to-pharmacy` - إرسال وصفة

### في الصيدلية - المنفذ 5001
- `POST /api/prescriptions` - إنشاء وصفة جديدة
- `GET /api/prescriptions` - جميع الوصفات
- `GET /api/prescriptions/search/:patientName` - البحث بالاسم
- `PUT /api/prescriptions/:id/status` - تحديث الحالة
- `GET /api/medications` - الأدوية المتاحة

## ملاحظات مهمة

- النظام مبسط ولا يحتوي على نظام API Token معقد
- يستخدم رمز العيادة (clinicCode) للتعريف
- جميع الطلبات مفتوحة (public) للسهولة
- يمكن تطوير النظام لاحقاً لإضافة المزيد من الأمان
- البيانات التجريبية تُضاف تلقائياً عند تشغيل `npm run setup`

## استكشاف الأخطاء

### إذا لم تعمل الطلبات:
1. تأكد من تشغيل كلا الخادمين
2. تأكد من صحة المنافذ (5000 للعيادة، 5001 للصيدلية)
3. تأكد من إضافة متغيرات البيئة في العيادة
4. تأكد من تشغيل `npm run setup` في الصيدلية

### إذا لم تظهر الأدوية:
1. تأكد من تشغيل `npm run setup` في الصيدلية
2. تأكد من الاتصال بقاعدة البيانات
3. تحقق من وجود البيانات في قاعدة البيانات
