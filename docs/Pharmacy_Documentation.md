# Pharmacy System API Documentation

## Overview
The Pharmacy System is a standalone microservice that manages medication inventory and prescription processing. It receives prescriptions from healthcare clinics and manages the complete medication dispensing workflow.

## Base URL
```
http://localhost:5001/api
```

## Authentication
This system uses a simplified approach without complex API tokens. It identifies clinics using clinic codes for prescription management.

---

## Prescription Management Endpoints

### Create New Prescription
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
- `patientId` (optional): Unique patient identifier
- `patientName` (required): Full name of the patient
- `patientAge` (optional): Age of the patient
- `patientPhone` (optional): Patient's phone number
- `doctorName` (required): Name of the prescribing doctor
- `doctorLicense` (optional): Doctor's license number
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
```

**Error Responses:**
- `400` - Missing required fields or invalid medication data
- `404` - Medication not found
- `400` - Insufficient stock for medication

### Get All Prescriptions
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
      "prescriptionNotes": "Patient has headache and mild infection",
      "prescriptionDate": "2024-01-15T10:00:00.000Z",
      "status": "pending",
      "totalCost": 56.00,
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

### Search Prescription by Patient Name
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
      "totalCost": 56.00,
      "prescriptionDate": "2024-01-15T10:00:00.000Z"
    }
  ]
}
```

**Error Responses:**
- `404` - No prescriptions found for this patient

### Update Prescription Status
```http
PUT /api/prescriptions/64f1a2b3c4d5e6f7g8h9i0j1/status
Content-Type: application/json

{
  "status": "ready",
  "notes": "Prescription is ready for pickup"
}
```

**Request Body Fields:**
- `status` (required): New status (pending, processing, ready, completed, cancelled)
- `notes` (optional): Additional notes about the status change

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

**Error Responses:**
- `400` - Status is required or invalid status
- `404` - Prescription not found

---

## Medication Management Endpoints

### Get Available Medications
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
      "category": "Antibiotics",
      "supplier": "MediCorp",
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

## Prescription Status Workflow

1. **pending** - Prescription received and waiting to be processed
2. **processing** - Prescription is being prepared
3. **ready** - Prescription is ready for pickup
4. **completed** - Prescription has been collected by patient
5. **cancelled** - Prescription has been cancelled

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
- `404` - Not Found
- `500` - Internal Server Error

---

## Environment Variables

Create a `.env` file with the following variables:

```env
# Server Configuration
PORT=5001
NODE_ENV=development

# Database
DB_URL=mongodb://localhost:27017/pharmacy_db

# JWT Configuration
JWT_SECRET=pharmacy_jwt_secret_here
JWT_REFRESH_SECRET=pharmacy_refresh_secret_here

# HMAC Configuration
HMAC_VERIFICATION_CODE_SECRET=pharmacy_hmac_secret_here

# Email Configuration
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=pharmacy@example.com
SMTP_PASS=pharmacy_password
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

3. **Setup sample data:**
   ```bash
   npm run setup
   ```

4. **Start the development server:**
   ```bash
   npm run dev
   ```

5. **The API will be available at:**
   ```
   http://localhost:5001/api
   ```

---

## Sample Data

The system comes with pre-loaded sample data:

### Medications:
- **Paracetamol 500mg** (15.50 SAR) - Pain reliever and fever reducer
- **Amoxicillin 250mg** (25.00 SAR) - Broad spectrum antibiotic
- **Ibuprofen 400mg** (18.75 SAR) - Anti-inflammatory and pain reliever
- **Omeprazole 20mg** (35.00 SAR) - Proton pump inhibitor for stomach ulcers
- **Loratadine 10mg** (22.50 SAR) - Antihistamine for allergy treatment

### Client:
- **Name**: Healthcare Care Clinic
- **Code**: CLINIC_001
- **Contact**: Dr. Ahmed Mohammed
- **Phone**: 0123456789
- **Address**: King Fahd Street, Riyadh, Saudi Arabia

---

## Workflow Example

1. **Clinic sends prescription:**
   ```http
   POST /api/prescriptions
   ```

2. **Pharmacy processes prescription:**
   ```http
   PUT /api/prescriptions/:id/status
   ```

3. **Patient searches for prescription:**
   ```http
   GET /api/prescriptions/search/John Doe
   ```

4. **Pharmacy updates status when ready:**
   ```http
   PUT /api/prescriptions/:id/status
   ```

5. **Patient collects medication:**
   ```http
   PUT /api/prescriptions/:id/status
   ```

---

## Security Features

- Input validation using Joi
- CORS protection
- Helmet for security headers
- Error handling without information leakage
- Clinic code-based identification system

---

## Testing

The API can be tested using tools like:
- Postman
- Insomnia
- curl
- Any HTTP client

All endpoints are public for simplicity in this MVP version.
