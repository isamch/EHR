# CareFlow EHR API Documentation

## Overview
CareFlow EHR is a comprehensive Electronic Health Record management system that integrates with external pharmacy services. It provides complete solutions for managing medical records, appointments, and patient care across multiple healthcare roles.

## Base URL
```
http://localhost:5000/api
```

## Authentication
The system uses JWT (JSON Web Tokens) for authentication with HTTP-only cookies for security. All protected routes require a valid JWT token in the Authorization header.

---

## Authentication Endpoints

### Register New Patient
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

**Error Responses:**
- `400` - Missing or invalid data
- `409` - Email already exists

### Login
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

**Error Responses:**
- `401` - Invalid credentials

### Refresh Token
```http
POST /api/auth/refresh
```

### Logout
```http
POST /api/auth/logout
```

### Verify Email
```http
GET /api/auth/verify-email/:token
```

### Forgot Password
```http
POST /api/auth/forgot-password
Content-Type: application/json

{
  "email": "john@example.com"
}
```

### Reset Password
```http
POST /api/auth/reset-password/:token
Content-Type: application/json

{
  "password": "newpassword123"
}
```

---

## Doctor Pharmacy Integration Endpoints

### Search Medications in Pharmacy
```http
GET /api/doctor/medications/search?search=paracetamol
Authorization: Bearer <token>
```

**Query Parameters:**
- `search` (optional): Search term for medication name

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

**Error Responses:**
- `401` - Unauthorized
- `500` - Pharmacy connection error

### Get Available Medications
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
        },
        {
          "id": "64f1a2b3c4d5e6f7g8h9i0j2",
          "name": "Amoxicillin 250mg",
          "code": "AMOX_250",
          "description": "Broad spectrum antibiotic",
          "stockQuantity": 50,
          "unit": "box",
          "price": 25.00,
          "requiresPrescription": true,
          "category": "Antibiotics"
        }
      ]
    }
  }
}
```

### Send Prescription to Pharmacy
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
    },
    {
      "medicationName": "Amoxicillin 250mg",
      "quantity": 1,
      "dosage": "250mg",
      "duration": "10 days",
      "notes": "Take with water"
    }
  ],
  "prescriptionNotes": "Patient has headache and mild infection"
}
```

**Request Body Fields:**
- `patientName` (required): Full name of the patient
- `patientAge` (optional): Age of the patient
- `patientPhone` (optional): Patient's phone number
- `doctorName` (required): Name of the prescribing doctor
- `clinicName` (required): Name of the clinic
- `clinicCode` (required): Unique clinic identifier
- `medications` (required): Array of medications
  - `medicationName` (required): Name of the medication
  - `quantity` (required): Number of units
  - `dosage` (required): Dosage information
  - `duration` (optional): Duration of treatment
  - `notes` (optional): Additional notes
- `prescriptionNotes` (optional): General prescription notes

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
      "totalCost": 56.00,
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
        },
        {
          "medicationId": "64f1a2b3c4d5e6f7g8h9i0j2",
          "medicationName": "Amoxicillin 250mg",
          "quantity": 1,
          "dosage": "250mg",
          "duration": "10 days",
          "notes": "Take with water",
          "price": 25.00,
          "totalPrice": 25.00
        }
      ]
    }
  }
}
```

**Error Responses:**
- `400` - Missing required fields or invalid medication data
- `401` - Unauthorized
- `500` - Pharmacy connection error

---

## User Management Endpoints

### Get Current User Profile
```http
GET /api/user/me
Authorization: Bearer <token>
```

**Response:**
```json
{
  "id": "64f1a2b3c4d5e6f7g8h9i0j1",
  "fullName": "John Doe",
  "email": "john@example.com",
  "role": {
    "id": "64f1a2b3c4d5e6f7g8h9i0j2",
    "name": "Patient"
  },
  "status": "active",
  "isEmailVerified": true,
  "createdAt": "2024-01-15T10:00:00.000Z",
  "updatedAt": "2024-01-15T10:00:00.000Z"
}
```

### Update User Profile
```http
PUT /api/user/me
Authorization: Bearer <token>
Content-Type: application/json

{
  "fullName": "John Doe Updated",
  "phone": "0123456789"
}
```

---

## Patient Endpoints

### Get Patient Appointments
```http
GET /api/patient/appointments
Authorization: Bearer <token>
```

### Create New Appointment
```http
POST /api/patient/appointments
Authorization: Bearer <token>
Content-Type: application/json

{
  "doctorId": "64f1a2b3c4d5e6f7g8h9i0j3",
  "startTime": "2024-01-15T10:00:00.000Z",
  "endTime": "2024-01-15T10:30:00.000Z",
  "reason": "Regular checkup"
}
```

### Cancel Appointment
```http
PATCH /api/patient/appointments/:id/cancel
Authorization: Bearer <token>
```

### Get Medical Record
```http
GET /api/patient/record/me
Authorization: Bearer <token>
```

---

## Doctor Endpoints

### Get Doctor Profile
```http
GET /api/doctor/profile/me
Authorization: Bearer <token>
```

### Update Doctor Profile
```http
PUT /api/doctor/profile/me
Authorization: Bearer <token>
Content-Type: application/json

{
  "specialization": "Cardiology",
  "workingHours": [
    {
      "dayOfWeek": "Monday",
      "timeSlots": [
        {
          "startTime": "09:00",
          "endTime": "17:00",
          "isAvailable": true
        }
      ]
    }
  ]
}
```

### Get Doctor Appointments
```http
GET /api/doctor/appointments/me
Authorization: Bearer <token>
```

### Update Appointment Status
```http
PATCH /api/doctor/appointments/:id/status
Authorization: Bearer <token>
Content-Type: application/json

{
  "status": "completed"
}
```

### Get Patient Record
```http
GET /api/doctor/patients/:patientId/record
Authorization: Bearer <token>
```

### Add Patient Visit
```http
POST /api/doctor/patients/:patientId/visits
Authorization: Bearer <token>
Content-Type: application/json

{
  "diagnosis": ["Hypertension", "Diabetes"],
  "symptoms": ["Headache", "Fatigue"],
  "treatments": [
    {
      "name": "Metformin",
      "dosage": "500mg",
      "duration": "30 days"
    }
  ],
  "notes": "Patient responding well to treatment"
}
```

---

## Nurse Endpoints

### Get Nurse Profile
```http
GET /api/nurse/profile/me
Authorization: Bearer <token>
```

### Get Nurse Appointments
```http
GET /api/nurse/appointments/me
Authorization: Bearer <token>
```

### Get Assigned Patients
```http
GET /api/nurse/patients/me
Authorization: Bearer <token>
```

---

## Secretary Endpoints

### Get Secretary Profile
```http
GET /api/secretary/profile/me
Authorization: Bearer <token>
```

### Get All Appointments
```http
GET /api/secretary/appointments
Authorization: Bearer <token>
```

### Get All Patients
```http
GET /api/secretary/patients
Authorization: Bearer <token>
```

---

## Admin Endpoints

### Get All Users
```http
GET /api/admin/users
Authorization: Bearer <token>
```

### Create New User
```http
POST /api/admin/users
Authorization: Bearer <token>
Content-Type: application/json

{
  "fullName": "Dr. Smith",
  "email": "dr.smith@example.com",
  "password": "password123",
  "roleId": "64f1a2b3c4d5e6f7g8h9i0j4"
}
```

### Update User
```http
PUT /api/admin/users/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "status": "active"
}
```

### Delete User
```http
DELETE /api/admin/users/:id
Authorization: Bearer <token>
```

### Get System Logs
```http
GET /api/admin/logs
Authorization: Bearer <token>
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

## Environment Variables

Create a `.env` file with the following variables:

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# Database
DB_URL=mongodb://localhost:27017/careflow_ehr

# JWT Configuration
JWT_SECRET=your_jwt_secret_here
JWT_REFRESH_SECRET=your_refresh_secret_here

# HMAC Configuration
HMAC_VERIFICATION_CODE_SECRET=your_hmac_secret_here

# Email Configuration
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=your@email.com
SMTP_PASS=your_password

# Pharmacy Integration
PHARMACY_API_URL=http://localhost:5001/api
CLINIC_CODE=CLINIC_001
CLINIC_NAME=Healthcare Care Clinic
```

---

## Getting Started

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Set up environment variables:**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Start the development server:**
   ```bash
   npm run dev
   ```

4. **The API will be available at:**
   ```
   http://localhost:5000/api
   ```

---

## Security Features

- JWT-based authentication with HTTP-only cookies
- Password hashing using bcryptjs
- CORS protection
- Helmet for security headers
- Input validation using Joi
- Role-based access control
- Email verification system
- Password reset functionality

---

## Testing

The API can be tested using tools like:
- Postman
- Insomnia
- curl
- Any HTTP client

Make sure to include the `Authorization: Bearer <token>` header for protected routes.
