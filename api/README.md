# API Documentation

## Overview

This document provides comprehensive documentation for the Recruitment Platform API. The API follows RESTful conventions and uses JSON for request/response bodies.

## Base URL

```
https://api.yourplatform.com/api
```

## Authentication

The API uses JWT (JSON Web Token) authentication. Include the token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

### Login

**POST** `/auth/login`

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": "user-123",
    "email": "user@example.com",
    "fullName": "John Doe",
    "role": "employer"
  }
}
```

## Endpoints

### Vacancies

#### Get All Vacancies

**GET** `/vacancies`

**Query Parameters:**
- `status` (optional): Filter by status (draft, published, paused, closed)
- `province` (optional): Filter by province
- `page` (optional): Page number (default: 1)
- `pageSize` (optional): Items per page (default: 10)

**Response:**
```json
{
  "data": [
    {
      "id": "vacancy-123",
      "title": "Software Engineer",
      "description": "We are looking for...",
      "location": "Ho Chi Minh City",
      "quantity": 5,
      "salaryMin": 1000,
      "salaryMax": 2000,
      "status": "published",
      "createdAt": "2024-01-01T00:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 10,
    "totalCount": 50,
    "totalPages": 5
  }
}
```

#### Create Vacancy

**POST** `/vacancies`

**Request Body:**
```json
{
  "title": "Senior Developer",
  "description": "Job description...",
  "location": "Ha Noi",
  "quantity": 3,
  "salaryMin": 1500,
  "salaryMax": 2500,
  "startDate": "2024-02-01",
  "applicationDeadline": "2024-01-15",
  "commissionRuleId": "rule-123"
}
```

#### Update Vacancy

**PUT** `/vacancies/{id}`

#### Delete Vacancy

**DELETE** `/vacancies/{id}`

### Applications

#### Get Applications

**GET** `/applications`

**Query Parameters:**
- `vacancyId` (optional): Filter by vacancy
- `status` (optional): Filter by status

**Response:**
```json
[
  {
    "id": "app-123",
    "vacancyId": "vacancy-123",
    "candidateId": "candidate-456",
    "recruiterId": "recruiter-789",
    "status": "applied",
    "appliedAt": "2024-01-01T00:00:00Z"
  }
]
```

#### Update Application Status

**PATCH** `/applications/{id}/status`

**Request Body:**
```json
{
  "status": "shortlisted"
}
```

### Commissions

#### Get Commissions

**GET** `/commissions`

**Query Parameters:**
- `status` (optional): Filter by status
- `recruiterId` (optional): Filter by recruiter

**Response:**
```json
[
  {
    "id": "commission-123",
    "applicationId": "app-123",
    "recruiterId": "recruiter-789",
    "amount": 1000000,
    "status": "pending",
    "createdAt": "2024-01-01T00:00:00Z"
  }
]
```

### AI Services

#### Match Candidate to Job

**POST** `/ai/match-candidate`

**Request Body:**
```json
{
  "candidateId": "candidate-456",
  "vacancyId": "vacancy-123"
}
```

**Response:**
```json
{
  "matches": [
    {
      "candidate_id": "candidate-456",
      "job_id": "vacancy-123",
      "overall_score": 0.85,
      "score_category": "very_good",
      "explanation": "Excellent skill match..."
    }
  ]
}
```

#### Parse CV

**POST** `/ai/parse-cv`

**Content-Type:** `multipart/form-data`

**Form Data:**
- `cv`: CV file (PDF, DOCX, or image)

**Response:**
```json
{
  "fullName": "Nguyen Van A",
  "email": "nguyenvana@example.com",
  "phone": "+84901234567",
  "skills": ["JavaScript", "React", "Node.js"],
  "confidence": 0.92
}
```

### Reports

#### Get KPI Data

**GET** `/reports/kpi`

**Query Parameters:**
- `recruiterId` (optional): Filter by recruiter
- `from` (optional): Start date (YYYY-MM-DD)
- `to` (optional): End date (YYYY-MM-DD)

**Response:**
```json
{
  "totalCVsSubmitted": 150,
  "totalContractsSigned": 12,
  "conversionRate": 0.08,
  "averageTimeToHire": 14.5,
  "totalEarnings": 15000000
}
```

## Error Handling

All API endpoints return appropriate HTTP status codes and error messages:

```json
{
  "message": "Validation failed",
  "errors": {
    "title": ["Title is required"],
    "salaryMin": ["Must be greater than 0"]
  }
}
```

**Common Status Codes:**
- `200`: Success
- `201`: Created
- `400`: Bad Request (validation errors)
- `401`: Unauthorized (invalid/missing token)
- `403`: Forbidden (insufficient permissions)
- `404`: Not Found
- `500`: Internal Server Error

## Rate Limiting

API endpoints are rate-limited to prevent abuse:

- **Authentication endpoints**: 5 requests per minute
- **General endpoints**: 100 requests per minute
- **File upload**: 10 requests per minute

Rate limit headers are included in responses:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1640995200
```

## Webhooks

### Payment Webhook

**POST** `/payments/webhook`

Handles payment status updates from bank APIs.

**Request Body:**
```json
{
  "eventType": "payment_status_update",
  "transactionId": "txn-123",
  "status": "completed",
  "amount": 1000000,
  "bankReference": "BANK-REF-456"
}
```

## SDKs and Libraries

### JavaScript/TypeScript SDK

```typescript
import { RecruitmentAPI } from '@yourplatform/sdk';

const api = new RecruitmentAPI({
  baseUrl: 'https://api.yourplatform.com/api',
  apiKey: 'your-api-key'
});

// Create vacancy
const vacancy = await api.vacancies.create({
  title: 'Senior Developer',
  description: 'Job description...',
  // ... other fields
});
```

### Python SDK

```python
from recruitment_sdk import RecruitmentAPI

api = RecruitmentAPI(
    base_url='https://api.yourplatform.com/api',
    api_key='your-api-key'
)

# Get vacancies
vacancies = api.get_vacancies(status='published')
```

## Support

For API support and questions:

- **Email**: api-support@yourplatform.com
- **Documentation**: https://docs.yourplatform.com/api
- **Status Page**: https://status.yourplatform.com
- **Community Forum**: https://community.yourplatform.com

## Changelog

### Version 1.0.0
- Initial API release
- Core vacancy and application management
- Commission tracking
- AI-powered matching
- Comprehensive logging and monitoring