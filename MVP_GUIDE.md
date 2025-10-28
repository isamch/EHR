# نظام MVP بسيط للتواصل بين CareFlow والصيدلية

## نظرة عامة
نظام بسيط للتواصل بين عيادة CareFlow والصيدلية لإدارة الوصفات الطبية.

## الخطوات المطلوبة

### 1. إعداد الصيدلية
```bash
cd pharmacy
npm install
# إضافة بيانات الأدوية في قاعدة البيانات
# إنشاء عميل جديد للعيادة
```

### 2. إعداد العيادة (CareFlow)
```bash
cd "CareFlow EHR"
npm install
# إضافة متغيرات البيئة في .env
```

### 3. متغيرات البيئة المطلوبة في العيادة
```env
PHARMACY_API_URL=http://localhost:5001/api
CLINIC_CODE=CLINIC_001
CLINIC_NAME=عيادة الرعاية الصحية
```

## كيفية الاستخدام

### 1. إنشاء حساب في الصيدلية
```javascript
// في قاعدة بيانات الصيدلية
const client = {
  name: "عيادة الرعاية الصحية",
  clinicCode: "CLINIC_001",
  contactPerson: "د. أحمد",
  phone: "0123456789",
  address: "شارع الملك فهد، الرياض"
}
```

### 2. البحث عن الأدوية
```http
GET /api/doctor/medications/search?search=باراسيتامول
```

### 3. إرسال وصفة طبية
```http
POST /api/doctor/prescriptions/send-to-pharmacy
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
      "medicationName": "باراسيتامول",
      "quantity": 2,
      "dosage": "500mg",
      "duration": "7 أيام",
      "notes": "يؤخذ بعد الأكل"
    }
  ],
  "prescriptionNotes": "مريض يعاني من صداع"
}
```

### 4. البحث عن وصفة في الصيدلية
```http
GET /api/prescriptions/search/أحمد محمد
```

### 5. تحديث حالة الوصفة
```http
PUT /api/prescriptions/:id/status
Content-Type: application/json

{
  "status": "ready",
  "notes": "الوصفة جاهزة للاستلام"
}
```

## API Endpoints

### في العيادة (CareFlow)
- `GET /api/doctor/medications/search` - البحث عن الأدوية
- `GET /api/doctor/medications/available` - الأدوية المتاحة
- `POST /api/doctor/prescriptions/send-to-pharmacy` - إرسال وصفة

### في الصيدلية
- `POST /api/prescriptions` - إنشاء وصفة جديدة
- `GET /api/prescriptions` - جميع الوصفات
- `GET /api/prescriptions/search/:patientName` - البحث بالاسم
- `PUT /api/prescriptions/:id/status` - تحديث الحالة
- `GET /api/medications` - الأدوية المتاحة

## تدفق العمل

1. **الطبيب يبحث عن الأدوية** في قاعدة بيانات الصيدلية
2. **الطبيب يختار الأدوية** المطلوبة للمريض
3. **الطبيب يرسل الوصفة** للصيدلية مع بيانات المريض
4. **الصيدلية تستقبل الوصفة** وتتحقق من توفر الأدوية
5. **الصيدلية تعد الأدوية** وتحدث حالة الوصفة
6. **المريض يذهب للصيدلية** ويبحث عن اسمه
7. **الصيدلية تعطي الدواء** للمريض وتحدث الحالة

## ملاحظات مهمة

- النظام مبسط ولا يحتوي على نظام API Token معقد
- يستخدم رمز العيادة (clinicCode) للتعريف
- جميع الطلبات مفتوحة (public) للسهولة
- يمكن تطوير النظام لاحقاً لإضافة المزيد من الأمان
