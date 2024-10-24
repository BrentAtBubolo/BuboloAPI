# Medical Practice API Documentation

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Authentication](#authentication)
- [Installation](#installation)
- [API Endpoints](#api-endpoints)
- [Error Handling](#error-handling)
- [Security](#security)
- [Development](#development)
- [Best Practices](#best-practices)

## Overview
Next.js API for medical practice management, connecting a Salesforce backend with a Flutter mobile application.

## Features

### 1. User Authentication
- Phone number-based authentication
- OTP verification
- JWT token management
- Session handling

### 2. User Profile Management
- View patient profile details
- Update personal information
- Contact information management
- Address management

### 3. Appointment Management
- View upcoming appointments
- View past appointments
- Book new appointments
- Cancel appointments
- Reschedule appointments
- Appointment reminders

### 4. Doctor Information
- View doctor profiles
- Filter by specialization
- Check doctor availability
- View consultation fees
- Doctor ratings and reviews
- Location information

### 5. Health Reports
- Health score calculation
- Blood pressure tracking
- Temperature records
- Weight monitoring
- Blood glucose levels
- Historical health data

### 6. Recent Checkups
- View recent medical checkups
- Blood test results
- Temperature records
- X-ray records
- Medical notes
- Visit summaries

### 7. Medical Services
- List of available services
- Service categories
- Service pricing
- Service descriptions
- Membership benefits
- Special offers

### 8. Prescription Management
- Active prescriptions list
- Prescription details
- Medication information
- Dosage instructions
- Refill requests
- Transfer requests
- Prescription history

### 9. Weight and Stress Monitoring
- Weight tracking
- BMI calculation
- Weight goals
- Progress tracking
- Stress level monitoring
- Historical data visualization

### 10. Notifications
- Appointment reminders
- Medication reminders
- Health tips
- Push notifications
- Email notifications
- SMS notifications
- Notification preferences

### 11. Medical History
- Past visits
- Treatment history
- Doctor observations
- Medical conditions
- Allergies
- Medications

### 12. Health Metrics Visualization
- Health trends
- Progress graphs
- Custom date ranges
- Data export
- Comparative analysis
- Monthly reports

## Installation

### Prerequisites
```bash
Node.js 18+ (LTS recommended)
npm or yarn
Git
```

### Environment Variables
Create a `.env.local` file:
```env
# Salesforce Configuration
SF_CLIENT_ID=your_client_id
SF_CLIENT_SECRET=your_client_secret
SF_USERNAME=your_username
SF_PASSWORD=your_password
SF_SECURITY_TOKEN=your_security_token

# JWT Configuration
JWT_SECRET=your_jwt_secret

# Twilio Configuration (for SMS)
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=your_phone_number

# Email Configuration
SMTP_HOST=your_smtp_host
SMTP_PORT=your_smtp_port
SMTP_USER=your_smtp_user
SMTP_PASS=your_smtp_pass

# API Configuration
ALLOWED_ORIGIN=your_frontend_domain
```

### Setup
```bash
# Clone the repository
git clone https://github.com/your-repo/medical-practice-api.git

# Install dependencies
cd medical-practice-api
npm install

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

## API Endpoints

### Authentication

#### Send OTP
```http
POST /api/auth/send-otp
Content-Type: application/json

{
    "phoneNumber": "+1234567890"
}
```

#### Verify OTP
```http
POST /api/auth/verify-otp
Content-Type: application/json

{
    "phoneNumber": "+1234567890",
    "otp": "123456"
}
```

### Profile Management

#### Get Profile
```http
GET /api/profile/{patientId}
Authorization: Bearer {token}
```

#### Update Profile
```http
PUT /api/profile/{patientId}
Authorization: Bearer {token}
Content-Type: application/json

{
    "firstName": "string",
    "lastName": "string",
    "phone": "string",
    "email": "string",
    "mailingStreet": "string",
    "mailingCity": "string",
    "mailingState": "string",
    "mailingPostalCode": "string"
}
```

### Appointments

#### Get Appointments
```http
GET /api/appointments/{patientId}
Authorization: Bearer {token}
```

#### Book Appointment
```http
POST /api/appointments
Authorization: Bearer {token}
Content-Type: application/json

{
    "serviceId": "string",
    "providerId": "string",
    "appointmentDateTime": "2024-01-01T10:00:00Z",
    "notes": "string"
}
```

#### Cancel Appointment
```http
DELETE /api/appointments/{appointmentId}
Authorization: Bearer {token}
```

#### Reschedule Appointment
```http
PUT /api/appointments/{appointmentId}
Authorization: Bearer {token}
Content-Type: application/json

{
    "newDateTime": "2024-01-01T11:00:00Z"
}
```

### Health Metrics

#### Get Health Score and Metrics
```http
GET /api/health-metrics/{patientId}
Authorization: Bearer {token}
Query Parameters:
    timeframe: '1m' | '3m' | '6m' | '1y'
```

Response:
```json
{
    "healthScore": 85,
    "metrics": {
        "currentWeight": "70 kg",
        "bloodPressure": "120/80",
        "bloodGlucose": {
            "fasting": "95 mg/dl",
            "postPrandial": "120 mg/dl"
        },
        "temperature": "98.6 F",
        "pulse": "72 bpm"
    },
    "trends": {
        "weight": {
            "trend": "declining",
            "changePercent": -2.5
        },
        "bloodPressure": {
            "trend": "stable",
            "average": "118/79"
        }
    },
    "historicalData": [
        {
            "date": "2024-01-01",
            "metrics": {
                // Daily metrics
            }
        }
    ]
}
```

#### Record New Metrics
```http
POST /api/health-metrics/{patientId}
Authorization: Bearer {token}
Content-Type: application/json

{
    "weight": "70",
    "bloodPressure": "120/80",
    "bloodGlucose": {
        "fasting": "95",
        "postPrandial": "120"
    },
    "temperature": "98.6",
    "pulse": "72"
}
```

### Doctor Information

#### Get Doctors List
```http
GET /api/doctors
Authorization: Bearer {token}
Query Parameters:
    specialty: string
    location: string
    availability: date
```

#### Get Doctor Details
```http
GET /api/doctors/{doctorId}
Authorization: Bearer {token}
```

#### Get Doctor Availability
```http
GET /api/doctors/{doctorId}/availability
Authorization: Bearer {token}
Query Parameters:
    date: YYYY-MM-DD
```

#### Get Doctor Reviews
```http
GET /api/doctors/{doctorId}/reviews
Authorization: Bearer {token}
```

### Medical Services

#### Get Available Services
```http
GET /api/services
Authorization: Bearer {token}
Query Parameters:
    category: string
```

#### Get Service Details
```http
GET /api/services/{serviceId}
Authorization: Bearer {token}
```

### Prescriptions

#### Get Active Prescriptions
```http
GET /api/prescriptions/{patientId}/active
Authorization: Bearer {token}
```

#### Get Prescription History
```http
GET /api/prescriptions/{patientId}/history
Authorization: Bearer {token}
```

#### Request Prescription Refill
```http
POST /api/prescriptions/refill
Authorization: Bearer {token}
Content-Type: application/json

{
    "prescriptionId": "string",
    "notes": "string",
    "urgent": boolean
}
```

#### Request Prescription Transfer
```http
POST /api/prescriptions/transfer
Authorization: Bearer {token}
Content-Type: application/json

{
    "prescriptionId": "string",
    "pharmacyName": "string",
    "pharmacyAddress": "string",
    "pharmacyPhone": "string"
}
```

### Medical History

#### Get Visit History
```http
GET /api/visits/{patientId}
Authorization: Bearer {token}
Query Parameters:
    from: YYYY-MM-DD
    to: YYYY-MM-DD
```

#### Get Visit Details
```http
GET /api/visits/{visitId}
Authorization: Bearer {token}
```

### Notifications

#### Update Notification Preferences
```http
PUT /api/notifications/preferences
Authorization: Bearer {token}
Content-Type: application/json

{
    "appointmentReminders": boolean,
    "prescriptionReminders": boolean,
    "healthTips": boolean,
    "emailNotifications": boolean,
    "smsNotifications": boolean,
    "reminderHours": number
}
```

#### Register Device for Push Notifications
```http
POST /api/notifications/register-device
Authorization: Bearer {token}
Content-Type: application/json

{
    "deviceToken": "string",
    "platform": "ios" | "android",
    "model": "string"
}
```

## Error Handling

All endpoints follow a consistent error response format:

```json
{
    "error": {
        "code": "string",
        "message": "string",
        "details": [
            {
                "field": "string",
                "message": "string"
            }
        ]
    }
}
```

### Common Error Codes
- `AUTH_001`: Authentication failed
- `AUTH_002`: Token expired
- `AUTH_003`: Invalid OTP
- `VAL_001`: Validation error
- `NOT_FOUND`: Resource not found
- `CONFLICT`: Resource conflict
- `SERVER_ERROR`: Internal server error

## Security

### Implementation Details

1. Authentication
```typescript
// middleware/auth.ts
export function withAuth(handler: NextApiHandler) {
    return async (req: NextApiRequest, res: NextApiResponse) => {
        try {
            // Token verification
            const token = req.headers.authorization?.split(' ')[1];
            if (!token) {
                return res.status(401).json({
                    error: {
                        code: 'AUTH_001',
                        message: 'No token provided'
                    }
                });
            }

            // Verify and decode token
            const decoded = verifyToken(token);

            // Add user info to request
            (req as AuthenticatedRequest).user = decoded;

            return handler(req as AuthenticatedRequest, res);
        } catch (error) {
            // Handle different types of auth errors
            return res.status(401).json({
                error: {
                    code: 'AUTH_002',
                    message: 'Authentication failed'
                }
            });
        }
    };
}
```

2. Rate Limiting
```typescript
// middleware/rateLimit.ts
export function withRateLimit(handler: NextApiHandler) {
    return async (req: NextApiRequest, res: NextApiResponse) => {
        const ip = req.headers['x-forwarded-for'] || req.socket.remoteAddress;
        const key = `rate-limit:${ip}`;
        
        // Implement rate limiting logic
    };
}
```

3. Input Validation
```typescript
// middleware/validate.ts
export function withValidation(schema: ZodSchema, handler: NextApiHandler) {
    return async (req: NextApiRequest, res: NextApiResponse) => {
        try {
            if (req.body) {
                await schema.parseAsync(req.body);
            }
            return handler(req, res);
        } catch (error) {
            return res.status(400).json({
                error: {
                    code: 'VAL_001',
                    message: 'Validation failed',
                    details: error.errors
                }
            });
        }
    };
}
```

## Development

### Project Structure
```
├── pages/
│   ├── api/
│   │   ├── auth/
│   │   ├── profile/
│   │   ├── appointments/
│   │   ├── health-metrics/
│   │   ├── doctors/
│   │   ├── services/
│   │   ├── prescriptions/
│   │   ├── visits/
│   │   └── notifications/
├── lib/
│   ├── salesforce.ts
│   ├── jwt.ts
│   ├── email.ts
│   ├── otp.ts
│   └── notifications.ts
├── middleware/
│   ├── auth.ts
│   ├── rateLimit.ts
│   └── validate.ts
├── types/
│   ├── api.ts
│   ├── salesforce.ts
│   └── schemas.ts
└── utils/
    ├── dates.ts
    ├── metrics.ts
    └── security.ts
```

### Running Tests
```bash
# Run all tests
npm test

# Run specific test suite
npm test appointments

# Run with coverage
npm run test:coverage
```

### Deployment

1. Vercel Deployment:
```bash
# Install Vercel CLI
npm i -g vercel

# Login to Vercel
vercel login

# Deploy
vercel
```

2. Environment Variables Setup:
- Go to Vercel Dashboard
- Select your project
- Navigate to Settings > Environment Variables
- Add all required environment variables

3. Production Checks:
```bash
# Build for production
npm run build

# Check for type errors
npm run type-check

# Run linting
npm run lint
```

## Best Practices

1. API Versioning
- Include version in URL: `/api/v1/...`
- Document breaking changes
- Maintain backward compatibility

2. Rate Limiting
- Implement per-endpoint limits
- Use sliding window algorithm
- Return remaining quota in headers

3. Caching
- Cache doctor/service lists
- Implement ETag support
- Use stale-while-revalidate

4. Error Handling
- Log errors with context
- Return consistent error format
- Include request ID in responses

5. Security
- Keep dependencies updated
- Implement CORS properly
- Use security headers
- Validate all input
- Sanitize all output

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License - see LICENSE.md

## Support

- Technical Support: tech-support@yourdomain.com
- API Issues: api-support@yourdomain.com
- Documentation: docs@yourdomain.com
