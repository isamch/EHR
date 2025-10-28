# CareFlow EHR & Pharmacy API - Postman Collection

This repository contains a comprehensive Postman collection for testing the CareFlow EHR system and Pharmacy integration API.

## 📁 Files Included

- **`CareFlow_API_Collection.postman_collection.json`** - Complete Postman collection with all endpoints
- **`CareFlow_Environment.postman_environment.json`** - Environment variables for different servers
- **`CareFlow_Test_Data.json`** - Sample data for all request bodies and test scenarios
- **`README.md`** - This documentation file

## 🚀 Quick Start

### 1. Import Collection and Environment

1. Open Postman
2. Click **Import** button
3. Import the following files:
   - `CareFlow_API_Collection.postman_collection.json`
   - `CareFlow_Environment.postman_environment.json`
4. Select the **CareFlow EHR & Pharmacy Environment** from the environment dropdown

### 2. Start Your Servers

Make sure both servers are running:

```bash
# EHR System (Port 5000)
cd "CareFlow EHR"
npm start

# Pharmacy System (Port 5001)
cd pharmacy
npm start
```

### 3. Test Authentication Flow

1. **Register a Patient**: Use `🔐 Authentication (EHR) > Register Patient`
2. **Login**: Use `🔐 Authentication (EHR) > Login`
3. **Test Protected Endpoints**: The access token will be automatically saved and used

## 📋 Collection Structure

### 🔐 Authentication (EHR)
- Register Patient
- Login
- Refresh Token
- Logout
- Verify Email
- Forgot Password
- Reset Password

### 👤 User Management
- Get My Profile

### 👨‍⚕️ Doctor Operations
- Get Doctor Profile
- Update Doctor Profile
- Get Doctor Appointments
- Update Appointment Status
- Get Patient Record
- Add Medical Visit
- Search Medications
- Get Available Medications
- Send Prescription to Pharmacy

### 🤒 Patient Operations
- Get Patient Appointments
- Create Appointment
- Cancel Appointment
- Get My Medical Record
- Get My Notifications

### 👩‍⚕️ Nurse Operations
- Get Nurse Profile
- Get Nurse Appointments
- Get Assigned Patients

### 📋 Secretary Operations
- Get Secretary Profile
- Get All Appointments
- Get All Patients

### ⚙️ Admin Operations
- Get All Users
- Get User by ID
- Get System Logs
- Get System Notifications

### 💊 Pharmacy Authentication
- Register Pharmacy User
- Pharmacy Login
- Pharmacy Refresh Token
- Pharmacy Logout
- Get Pharmacy Profile
- Update Pharmacy Profile

### 👥 Pharmacy Admin Management
- Get All Pharmacy Users
- Create Pharmacy User
- Get Pharmacy User Stats
- Get Pharmacy User by ID
- Update Pharmacy User
- Delete Pharmacy User
- Change Pharmacy User Password

### 💊 Medication Management
- Get All Medications
- Create Medication
- Get Medication Stats
- Get Low Stock Medications
- Get Medication by ID
- Update Medication
- Delete Medication
- Update Medication Stock

### 📋 Prescription Management
- Create Prescription
- Get All Prescriptions
- Search Prescriptions by Patient
- Update Prescription Status

## 🔧 Environment Variables

The collection uses the following environment variables:

### Server URLs
- `baseUrl`: http://localhost:5000/api (EHR System)
- `pharmacyBaseUrl`: http://localhost:5001/api (Pharmacy System)

### Authentication Tokens
- `accessToken`: JWT access token for EHR system
- `refreshToken`: JWT refresh token for EHR system
- `pharmacyAccessToken`: JWT access token for Pharmacy system
- `pharmacyRefreshToken`: JWT refresh token for Pharmacy system

### Test Data IDs
- `userId`: Current user ID
- `pharmacyUserId`: Current pharmacy user ID
- `patientId`: Patient ID for testing
- `doctorId`: Doctor ID for testing
- `appointmentId`: Appointment ID for testing
- `medicationId`: Medication ID for testing
- `prescriptionId`: Prescription ID for testing

### Test Credentials
- `testPatientEmail`: john.doe@example.com
- `testPatientPassword`: password123
- `testDoctorEmail`: doctor@example.com
- `testDoctorPassword`: doctor123
- `testPharmacyEmail`: admin@pharmacy.com
- `testPharmacyPassword`: pharmacy123
- `testNurseEmail`: nurse@example.com
- `testNursePassword`: nurse123
- `testSecretaryEmail`: secretary@example.com
- `testSecretaryPassword`: secretary123
- `testAdminEmail`: admin@example.com
- `testAdminPassword`: admin123

## 🧪 Test Scripts

Each request includes automated test scripts that:

1. **Extract Tokens**: Automatically save access and refresh tokens
2. **Extract IDs**: Save created resource IDs for subsequent requests
3. **Validate Responses**: Check response status, structure, and data
4. **Performance Tests**: Verify response times are acceptable

### Example Test Script:
```javascript
if (pm.response.code === 200) {
    const response = pm.response.json();
    if (response.data && response.data.accessToken) {
        pm.collectionVariables.set('accessToken', response.data.accessToken);
        pm.collectionVariables.set('refreshToken', response.data.refreshToken);
    }
    pm.test('Login successful', function () {
        pm.expect(response.status).to.eql('success');
        pm.expect(response.data.accessToken).to.exist;
        pm.expect(response.data.refreshToken).to.exist;
    });
}
```

## 📊 Sample Data

The `CareFlow_Test_Data.json` file contains comprehensive sample data for:

### User Data
- Patient information
- Doctor profiles
- Nurse details
- Secretary information
- Admin accounts
- Pharmacy users

### Medical Data
- Appointments
- Medical visits
- Vital signs
- Diagnoses
- Treatments

### Pharmacy Data
- Medications
- Prescriptions
- Stock updates
- Status changes

### Search Queries
- Medication searches
- Patient searches
- Filter options

## 🔄 Workflow Examples

### Complete Patient Journey
1. Register Patient
2. Login
3. Create Appointment
4. Get My Medical Record
5. Get My Notifications

### Doctor Workflow
1. Login as Doctor
2. Get Doctor Profile
3. Get Doctor Appointments
4. Get Patient Record
5. Add Medical Visit
6. Send Prescription to Pharmacy

### Pharmacy Workflow
1. Login to Pharmacy System
2. Get All Prescriptions
3. Search Prescriptions by Patient
4. Update Prescription Status
5. Manage Medication Stock

### Admin Workflow
1. Login as Admin
2. Get All Users
3. Get System Logs
4. Get System Notifications
5. Manage Pharmacy Users

## 🛠️ Customization

### Adding New Endpoints
1. Create new request in appropriate folder
2. Add proper headers and authentication
3. Include sample request body
4. Add test scripts for validation
5. Update environment variables if needed

### Modifying Test Data
1. Edit `CareFlow_Test_Data.json`
2. Update request bodies in collection
3. Adjust test scripts accordingly

### Environment Configuration
1. Modify `CareFlow_Environment.postman_environment.json`
2. Add new variables as needed
3. Update server URLs for different environments

## 🐛 Troubleshooting

### Common Issues

1. **Authentication Errors**
   - Ensure tokens are properly saved
   - Check token expiration
   - Verify login credentials

2. **Server Connection Issues**
   - Confirm servers are running
   - Check port numbers (5000 for EHR, 5001 for Pharmacy)
   - Verify server URLs in environment

3. **Missing IDs**
   - Run requests in proper sequence
   - Check that previous requests completed successfully
   - Verify test scripts are extracting IDs correctly

4. **Validation Errors**
   - Check request body format
   - Verify required fields are included
   - Ensure data types are correct

### Debug Tips

1. **Check Console**: Look at Postman console for detailed logs
2. **Verify Variables**: Check environment variables are set correctly
3. **Test Scripts**: Review test script output for errors
4. **Response Format**: Ensure responses match expected format

## 📈 Performance Testing

The collection includes performance tests that verify:
- Response times are under 5 seconds
- Responses have valid JSON structure
- Authentication tokens are properly managed

## 🔒 Security Features

- JWT token management
- Automatic token refresh
- Secure credential storage
- Role-based access testing
- Input validation testing

## 📝 Notes

- All timestamps use ISO 8601 format
- Passwords should be at least 6 characters
- Email addresses must be valid format
- IDs are automatically generated by the system
- Pagination is supported for list endpoints

## 🤝 Contributing

To contribute to this collection:

1. Test new endpoints thoroughly
2. Add comprehensive test scripts
3. Include sample data
4. Update documentation
5. Follow existing naming conventions

## 📞 Support

For issues or questions:
- Check the API documentation
- Review server logs
- Test with curl/other tools
- Contact development team

---

**Happy Testing! 🚀**
