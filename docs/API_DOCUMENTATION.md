# CareFlow EHR & Pharmacy Integration API Documentation

## Overview
This is a simple MVP system for integrating a healthcare clinic (CareFlow EHR) with a pharmacy. The system allows doctors to search for medications and send prescriptions to the pharmacy, while patients can collect their medications from the pharmacy.

## System Architecture

### CareFlow EHR (Port 5000)
- Main healthcare management system
- Handles patient records, appointments, and prescriptions
- Integrates with pharmacy for medication management

### Pharmacy System (Port 5001)
- Standalone pharmacy management system
- Manages medication inventory and prescription processing
- Receives prescriptions from clinics

## Base URLs
- **CareFlow EHR**: `http://localhost:5000/api`
- **Pharmacy**: `http://localhost:5001/api`

## Authentication
Both systems use JWT authentication with HTTP-only cookies. All protected routes require a valid JWT token in the Authorization header.

---

## CareFlow EHR API Endpoints

### Authentication Routes

#### Register Patient
```http
POST /api/auth/register
Content-Type: application/json

{
  "fullName": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Registration successful. Please check your email.",
  "data": {
    "userId": "64f1a2b3c4d5e6f7g8h9i0j1"
  }
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Login successful",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 900000
  }
}
```

### Doctor Pharmacy Integration Routes

#### Search Medications
```http
GET /api/doctor/medications/search?search=paracetamol
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "message": "Search completed successfully",
  "data": {
    "status": "success",
    "message": "Medications retrieved successfully",
    "data": {
      "total": 1,
      "page": 1,
      "perPage": 10,
      "data": [
        {
          "id": "64f1a2b3c4d5e6f7g8h9i0j1",
          "name": "Paracetamol 500mg",
          "code": "PARA_500",
          "description": "Pain reliever and fever reducer",
          "stockQuantity": 100,
          "unit": "box",
          "price": 15.50,
          "requiresPrescription": true,
          "category": "Analgesics"
        }
      ]
    }
  }
}
```

#### Get Available Medications
```http
GET /api/doctor/medications/available
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "message": "Available medications retrieved successfully",
  "data": {
    "status": "success",
    "message": "Medications retrieved successfully",
    "data": {
      "total": 5,
      "page": 1,
      "perPage": 10,
      "data": [
        {
          "id": "64f1a2b3c4d5e6f7g8h9i0j1",
          "name": "Paracetamol 500mg",
          "code": "PARA_500",
          "description": "Pain reliever and fever reducer",
          "stockQuantity": 100,
          "unit": "box",
          "price": 15.50,
          "requiresPrescription": true,
          "category": "Analgesics"
        }
      ]
    }
  }
}
```

#### Send Prescription to Pharmacy
```http
POST /api/doctor/prescriptions/send-to-pharmacy
Authorization: Bearer <token>
Content-Type: application/json

{
  "patientName": "John Doe",
  "patientAge": 30,
  "patientPhone": "0123456789",
  "doctorName": "Dr. Sarah Ahmed",
  "clinicName": "Healthcare Care Clinic",
  "clinicCode": "CLINIC_001",
  "medications": [
    {
      "medicationName": "Paracetamol 500mg",
      "quantity": 2,
      "dosage": "500mg",
      "duration": "7 days",
      "notes": "Take after meals"
    }
  ],
  "prescriptionNotes": "Patient has headache"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Prescription sent to pharmacy successfully",
  "data": {
    "status": "success",
    "message": "Prescription created successfully",
    "data": {
      "prescriptionId": "64f1a2b3c4d5e6f7g8h9i0j1",
      "status": "pending",
      "totalCost": 31.00,
      "estimatedReadyTime": "2024-01-15T10:30:00.000Z",
      "medications": [
        {
          "medicationId": "64f1a2b3c4d5e6f7g8h9i0j1",
          "medicationName": "Paracetamol 500mg",
          "quantity": 2,
          "dosage": "500mg",
          "duration": "7 days",
          "notes": "Take after meals",
          "price": 15.50,
          "totalPrice": 31.00
        }
      ]
    }
  }
}
```

---

## Pharmacy API Endpoints

### Prescription Management

#### Create Prescription
```http
POST /api/prescriptions
Content-Type: application/json

{
  "patientId": "patient_123",
  "patientName": "John Doe",
  "patientAge": 30,
  "patientPhone": "0123456789",
  "doctorName": "Dr. Sarah Ahmed",
  "doctorLicense": "MD123456",
  "clinicName": "Healthcare Care Clinic",
  "clinicCode": "CLINIC_001",
  "medications": [
    {
      "medicationName": "Paracetamol 500mg",
      "quantity": 2,
      "dosage": "500mg",
      "duration": "7 days",
      "notes": "Take after meals"
    }
  ],
  "prescriptionNotes": "Patient has headache"
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Prescription created successfully",
  "data": {
    "prescriptionId": "64f1a2b3c4d5e6f7g8h9i0j1",
    "status": "pending",
    "totalCost": 31.00,
    "estimatedReadyTime": "2024-01-15T10:30:00.000Z",
    "medications": [
      {
        "medicationId": "64f1a2b3c4d5e6f7g8h9i0j1",
        "medicationName": "Paracetamol 500mg",
        "quantity": 2,
        "dosage": "500mg",
        "duration": "7 days",
        "notes": "Take after meals",
        "price": 15.50,
        "totalPrice": 31.00
      }
    ]
  }
}
```

#### Get All Prescriptions
```http
GET /api/prescriptions
```

**Query Parameters:**
- `status` (optional): Filter by status (pending, processing, ready, completed, cancelled)
- `patientName` (optional): Filter by patient name
- `clinicCode` (optional): Filter by clinic code

**Response:**
```json
{
  "status": "success",
  "message": "Prescriptions retrieved successfully",
  "data": [
    {
      "id": "64f1a2b3c4d5e6f7g8h9i0j1",
      "prescriptionId": "PRESC_1705123456789_abc123def",
      "patientId": "patient_123",
      "patientName": "John Doe",
      "patientAge": 30,
      "patientPhone": "0123456789",
      "doctorName": "Dr. Sarah Ahmed",
      "doctorLicense": "MD123456",
      "clinicName": "Healthcare Care Clinic",
      "clinicCode": "CLINIC_001",
      "medications": [
        {
          "medicationId": "64f1a2b3c4d5e6f7g8h9i0j1",
          "medicationName": "Paracetamol 500mg",
          "quantity": 2,
          "dosage": "500mg",
          "duration": "7 days",
          "notes": "Take after meals",
          "price": 15.50,
          "totalPrice": 31.00
        }
      ],
      "prescriptionNotes": "Patient has headache",
      "prescriptionDate": "2024-01-15T10:00:00.000Z",
      "status": "pending",
      "totalCost": 31.00,
      "notes": null,
      "processedBy": null,
      "processedAt": null,
      "readyAt": null,
      "completedAt": null,
      "createdAt": "2024-01-15T10:00:00.000Z",
      "updatedAt": "2024-01-15T10:00:00.000Z"
    }
  ]
}
```

#### Search Prescription by Patient Name
```http
GET /api/prescriptions/search/John Doe
```

**Response:**
```json
{
  "status": "success",
  "message": "Prescriptions found",
  "data": [
    {
      "id": "64f1a2b3c4d5e6f7g8h9i0j1",
      "prescriptionId": "PRESC_1705123456789_abc123def",
      "patientName": "John Doe",
      "status": "pending",
      "medications": [
        {
          "medicationId": {
            "name": "Paracetamol 500mg",
            "code": "PARA_500"
          },
          "medicationName": "Paracetamol 500mg",
          "quantity": 2,
          "dosage": "500mg"
        }
      ],
      "totalCost": 31.00,
      "prescriptionDate": "2024-01-15T10:00:00.000Z"
    }
  ]
}
```

#### Update Prescription Status
```http
PUT /api/prescriptions/64f1a2b3c4d5e6f7g8h9i0j1/status
Content-Type: application/json

{
  "status": "ready",
  "notes": "Prescription is ready for pickup"
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Prescription status updated successfully",
  "data": {
    "prescriptionId": "64f1a2b3c4d5e6f7g8h9i0j1",
    "status": "ready",
    "notes": "Prescription is ready for pickup"
  }
}
```

### Medication Management

#### Get Available Medications
```http
GET /api/medications
```

**Query Parameters:**
- `search` (optional): Search by medication name or code
- `category` (optional): Filter by medication category

**Response:**
```json
{
  "status": "success",
  "message": "Medications retrieved successfully",
  "data": [
    {
      "id": "64f1a2b3c4d5e6f7g8h9i0j1",
      "name": "Paracetamol 500mg",
      "code": "PARA_500",
      "description": "Pain reliever and fever reducer",
      "stockQuantity": 100,
      "unit": "box",
      "price": 15.50,
      "requiresPrescription": true,
      "category": "Analgesics",
      "supplier": "PharmaCorp",
      "createdAt": "2024-01-15T10:00:00.000Z",
      "updatedAt": "2024-01-15T10:00:00.000Z"
    }
  ]
}
```

---

## Data Models

### Prescription Model
```json
{
  "id": "ObjectId",
  "prescriptionId": "String (unique)",
  "patientId": "String",
  "patientName": "String",
  "patientAge": "Number",
  "patientPhone": "String",
  "doctorName": "String",
  "doctorLicense": "String",
  "clinicName": "String",
  "clinicCode": "String",
  "medications": [
    {
      "medicationId": "ObjectId",
      "medicationName": "String",
      "quantity": "Number",
      "dosage": "String",
      "duration": "String",
      "notes": "String",
      "price": "Number",
      "totalPrice": "Number"
    }
  ],
  "prescriptionNotes": "String",
  "prescriptionDate": "Date",
  "status": "String (pending|processing|ready|completed|cancelled)",
  "totalCost": "Number",
  "notes": "String",
  "processedBy": "ObjectId",
  "processedAt": "Date",
  "readyAt": "Date",
  "completedAt": "Date",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

### Medication Model
```json
{
  "id": "ObjectId",
  "name": "String (unique)",
  "code": "String (unique)",
  "description": "String",
  "stockQuantity": "Number",
  "unit": "String",
  "price": "Number",
  "requiresPrescription": "Boolean",
  "category": "String",
  "supplier": "String",
  "lastUpdatedBy": "ObjectId",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

### Client Model
```json
{
  "id": "ObjectId",
  "name": "String",
  "clinicCode": "String (unique)",
  "contactPerson": "String",
  "phone": "String",
  "address": "String",
  "status": "String (active|inactive)",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

---

## Error Handling

All endpoints return consistent error responses:

```json
{
  "status": "error",
  "message": "Error description",
  "stack": null
}
```

### Common HTTP Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `409` - Conflict
- `500` - Internal Server Error

---

## Setup Instructions

### 1. Install Dependencies
```bash
# For CareFlow EHR
cd "CareFlow EHR"
npm install

# For Pharmacy
cd pharmacy
npm install
```

### 2. Environment Variables

**CareFlow EHR (.env):**
```env
PORT=5000
NODE_ENV=development
DB_URL=mongodb://localhost:27017/careflow_ehr
JWT_SECRET=your_jwt_secret_here
JWT_REFRESH_SECRET=your_refresh_secret_here
HMAC_VERIFICATION_CODE_SECRET=your_hmac_secret_here
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=your@email.com
SMTP_PASS=your_password
PHARMACY_API_URL=http://localhost:5001/api
CLINIC_CODE=CLINIC_001
CLINIC_NAME=Healthcare Care Clinic
```

**Pharmacy (.env):**
```env
PORT=5001
NODE_ENV=development
DB_URL=mongodb://localhost:27017/pharmacy_db
JWT_SECRET=pharmacy_jwt_secret_here
JWT_REFRESH_SECRET=pharmacy_refresh_secret_here
HMAC_VERIFICATION_CODE_SECRET=pharmacy_hmac_secret_here
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=pharmacy@example.com
SMTP_PASS=pharmacy_password
```

### 3. Setup Database
```bash
# Start MongoDB
mongod

# Setup pharmacy with sample data
cd pharmacy
npm run setup
```

### 4. Start Servers
```bash
# Start CareFlow EHR (Terminal 1)
cd "CareFlow EHR"
npm run dev

# Start Pharmacy (Terminal 2)
cd pharmacy
npm run dev
```

---

## Workflow Example

1. **Doctor searches for medications:**
   ```http
   GET /api/doctor/medications/search?search=paracetamol
   ```

2. **Doctor sends prescription:**
   ```http
   POST /api/doctor/prescriptions/send-to-pharmacy
   ```

3. **Pharmacy processes prescription:**
   ```http
   PUT /api/prescriptions/:id/status
   ```

4. **Patient collects medication:**
   ```http
   GET /api/prescriptions/search/John Doe
   ```

---

## Sample Data

The system comes with pre-loaded sample data:

### Medications:
- Paracetamol 500mg (15.50 SAR)
- Amoxicillin 250mg (25.00 SAR)
- Ibuprofen 400mg (18.75 SAR)
- Omeprazole 20mg (35.00 SAR)
- Loratadine 10mg (22.50 SAR)

### Client:
- Name: Healthcare Care Clinic
- Code: CLINIC_001
- Contact: Dr. Ahmed Mohammed
- Phone: 0123456789
