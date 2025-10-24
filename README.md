# 🚀 Recruitment Platform - Full Stack Project

[![.NET Core](https://img.shields.io/badge/.NET%20Core-8.0-purple.svg)](https://dotnet.microsoft.com/)
[![React](https://img.shields.io/badge/React-18.0-blue.svg)](https://reactjs.org/)
[![Flutter](https://img.shields.io/badge/Flutter-3.0+-blue.svg)](https://flutter.dev/)
[![SQL Server](https://img.shields.io/badge/SQL%20Server-2022-red.svg)](https://www.microsoft.com/en-us/sql-server)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)

> **Modern recruitment platform** connecting employers with recruiters through intelligent automation, real-time collaboration, and performance-driven incentives.

## 📋 Table of Contents

- [🏗️ Project Overview](#-project-overview)
- [🏛️ Architecture](#-architecture)
- [📁 Project Structure](#-project-structure)
- [🚀 Quick Start](#-quick-start)
- [🔧 Backend Development](#-backend-development)
- [🎨 Frontend Development](#-frontend-development)
- [🤖 AI Services](#-ai-services)
- [🏗️ Infrastructure](#-infrastructure)
- [📊 Database](#-database)
- [🔒 Security](#-security)
- [🧪 Testing](#-testing)
- [📦 Deployment](#-deployment)
- [🔍 Monitoring](#-monitoring)
- [👥 Team](#-team)

## 🏗️ Project Overview

### **Vision**
Transform traditional recruitment into an intelligent, gamified platform that makes hiring faster, more transparent, and more engaging for all participants.

### **Key Features**
- ✅ **Smart Job-Candidate Matching** - AI-powered recommendations
- ✅ **Real-time Collaboration** - Chat, notifications, live updates
- ✅ **Automated Commission Tracking** - Flexible payment rules
- ✅ **Performance Analytics** - KPI dashboards and gamification
- ✅ **Mobile-First Design** - Flutter apps for recruiters
- ✅ **Enterprise Security** - Bank-grade encryption and compliance

### **Target Users**
- **Employers**: HR managers, factory owners, company recruiters
- **Freelance Recruiters**: Individual recruiters and agencies
- **System Admins**: Platform management and finance oversight
- **Interviewers**: Company staff conducting interviews

## 🏛️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          Internet Users                                 │
├─────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │  React Web  │  │ Flutter App │  │   Admin     │  │   Public    │     │
│  │   Portal    │  │  (Mobile)   │  │   Panel     │  │    API      │     │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘     │
├─────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │   API       │  │  Real-time  │  │   File      │  │   Payment   │     │
│  │  Gateway    │  │   Service   │  │  Storage    │  │  Processor  │     │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘     │
├─────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │   .NET      │  │   AI/ML     │  │ Background  │  │   Cache     │     │
│  │   Core      │  │  Services   │  │   Jobs      │  │  (Redis)    │     │
│  │   APIs      │  │             │  │             │  │             │     │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘     │
├─────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌────────────────┐  │
│  │   SQL       │  │   File      │  │ Message     │  │   Search       │  │
│  │  Server     │  │  Storage    │  │  Queue      │  │   Engine       │  │
│  │  Database   │  │  (MinIO)    │  │ (RabbitMQ)  │  │ (Elasticsearch)│  │
│  └─────────────┘  └─────────────┘  └─────────────┘  └────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
                                Infrastructure Layer
```

## 📁 Project Structure

```
recruitment-platform/
├── 📁 backend/                          # .NET Core APIs
│   ├── 📁 src/
│   │   ├── 📁 Recruitment.API/          # Main API Gateway
│   │   ├── 📁 Recruitment.Application/  # Application Services
│   │   ├── 📁 Recruitment.Domain/       # Domain Models & Logic
│   │   ├── 📁 Recruitment.Infrastructure/ # Data Access & External Services
│   │   ├── 📁 Recruitment.Common/       # Shared Utilities
│   │   └── 📁 Recruitment.Tests/        # Unit & Integration Tests
│   ├── 📁 scripts/                      # Database & Deployment Scripts
│   └── 📁 docs/                         # API Documentation
├── 📁 frontend/                         # Web & Mobile Applications
│   ├── 📁 web/                          # React Employer Portal
│   │   ├── 📁 public/
│   │   ├── 📁 src/
│   │   │   ├── 📁 components/           # Reusable UI Components
│   │   │   ├── 📁 pages/               # Page Components
│   │   │   ├── 📁 services/            # API Services
│   │   │   ├── 📁 hooks/               # Custom React Hooks
│   │   │   ├── 📁 utils/               # Utility Functions
│   │   │   └── 📁 styles/              # Global Styles
│   │   ├── 📁 build/                   # Build Output
│   │   └── 📁 docs/                    # Storybook Documentation
│   ├── 📁 mobile/                       # Flutter Recruiter App
│   │   ├── 📁 lib/
│   │   │   ├── 📁 models/               # Data Models
│   │   │   ├── 📁 services/            # API Services
│   │   │   ├── 📁 ui/                  # UI Components
│   │   │   ├── 📁 utils/               # Utilities
│   │   │   └── 📁 providers/           # State Management
│   │   ├── 📁 android/                 # Android Platform
│   │   ├── 📁 ios/                     # iOS Platform
│   │   ├── 📁 build/                   # Build Output
│   │   └── 📁 docs/                    # App Documentation
│   └── 📁 admin/                        # Admin Management Portal
├── 📁 ai-services/                      # AI & ML Servicesrea
│   ├── 📁 cv-parser/                    # CV/Resume Parsing
│   ├── 📁 job-matcher/                  # Job-Candidate Matching
│   ├── 📁 kpi-analytics/                # Performance Analytics
│   ├── 📁 chat-bot/                     # Recruitment Assistant
│   └── 📁 models/                       # ML Models
├── 📁 infrastructure/                   # DevOps & Infrastructure
│   ├── 📁 docker/                       # Docker Configuration
│   │   ├── 📁 backend/                  # Backend Services
│   │   ├── 📁 database/                 # SQL Server
│   │   ├── 📁 ai-services/              # AI Service Containers
│   │   └── 📁 monitoring/               # Monitoring Stack
│   ├── 📁 k8s/                         # Kubernetes Manifests
│   ├── 📁 terraform/                    # Infrastructure as Code
│   └── 📁 scripts/                      # Deployment Scripts
├── 📁 database/                         # Database Scripts
│   ├── 📁 migrations/                   # Database Migrations
│   ├── 📁 seeds/                       # Sample Data
│   └── 📁 docs/                        # Database Documentation
├── 📁 docs/                            # Project Documentation
│   ├── 📁 api/                         # API Documentation
│   ├── 📁 architecture/                # Architecture Decisions
│   ├── 📁 deployment/                  # Deployment Guides
│   └── 📁 user-guides/                 # User Manuals
└── 📁 tools/                           # Development Tools
    ├── 📁 scripts/                     # Build & Dev Scripts
    └── 📁 config/                      # Configuration Files
```

## 🚀 Quick Start

### **Prerequisites**
- **Docker & Docker Compose** (for containerized deployment)
- **Node.js 18+** (for frontend development)
- **.NET 8 SDK** (for backend development)
- **Flutter SDK** (for mobile development)
- **SQL Server 2022** (or Docker image)

### **One-Command Setup**
```bash
# Clone the repository
git clone <repository-url>
cd recruitment-platform

# Start all services with Docker Compose
docker-compose -f infrastructure/docker/docker-compose.yml up -d

# Run database migrations
cd backend/scripts
./migrate-database.sh

# Start backend development server
cd backend/src
dotnet run --project Recruitment.API

# Start frontend development servers (in separate terminals)
cd frontend/web && npm start
cd frontend/mobile && flutter run
```

**Access the application:**
- **Employer Portal**: http://localhost:3000
- **Recruiter App**: Install from `frontend/mobile/build`
- **Admin Panel**: http://localhost:3001
- **API Documentation**: http://localhost:5000/swagger

## 🔧 Backend Development

### **Technology Stack**
- **Framework**: .NET 8 with ASP.NET Core Web API
- **Architecture**: Clean Architecture (Domain-Driven Design)
- **Database**: SQL Server with Entity Framework Core
- **Authentication**: JWT Bearer + Refresh Tokens
- **Documentation**: Swagger/OpenAPI 3.0

### **Project Structure**

```
backend/src/
├── Recruitment.API/              # API Gateway & Controllers
│   ├── Controllers/              # REST API Controllers
│   ├── Middleware/               # Custom Middleware
│   ├── Authentication/           # JWT & Auth Services
│   └── Startup.cs               # Application Configuration
├── Recruitment.Application/      # Use Cases & Business Logic
│   ├── Services/                 # Application Services
│   ├── DTOs/                     # Data Transfer Objects
│   ├── Interfaces/               # Service Interfaces
│   └── Validators/               # Input Validation
├── Recruitment.Domain/           # Business Rules & Models
│   ├── Entities/                 # Domain Entities
│   ├── ValueObjects/             # Immutable Values
│   ├── Aggregates/               # Entity Aggregates
│   └── Events/                   # Domain Events
├── Recruitment.Infrastructure/   # External Concerns
│   ├── Data/                     # Entity Framework DbContext
│   ├── Repositories/             # Data Access Layer
│   ├── Services/                 # External Service Integrations
│   └── Migrations/               # Database Migrations
└── Recruitment.Tests/            # Test Suites
    ├── Unit/                     # Unit Tests
    ├── Integration/              # Integration Tests
    └── Load/                     # Performance Tests
```

### **Key Development Commands**

```bash
# Install dependencies
dotnet restore

# Run tests
dotnet test

# Run with hot reload
dotnet watch run

# Create new migration
dotnet ef migrations add <MigrationName>

# Update database
dotnet ef database update

# Generate API client
dotnet swagger tofile --output ../frontend/web/src/services/api.json
```

### **API Structure**

```
API Routes:
├── /api/auth                    # Authentication endpoints
├── /api/vacancies              # Job posting management
├── /api/applications           # Application workflow
├── /api/interviews             # Interview scheduling
├── /api/contracts              # Contract management
├── /api/commissions            # Commission tracking
├── /api/payments               # Payment processing
├── /api/reports                # Analytics & reports
├── /api/admin                  # Administrative functions
└── /api/ai                     # AI service endpoints
```

## 🎨 Frontend Development

### **Web Portal (React)**

#### **Technology Stack**
- **React 18** with TypeScript
- **State Management**: Redux Toolkit + React Query
- **UI Framework**: Material-UI (MUI) + Custom Components
- **Routing**: React Router v6
- **Forms**: React Hook Form + Zod Validation
- **Real-time**: Socket.IO Client

#### **Key Features**
- **Dashboard Views**: KPI metrics, charts, leaderboards
- **Job Management**: Create, edit, track job postings
- **Candidate Pipeline**: Drag-drop Kanban interface
- **Interview Scheduling**: Calendar integration
- **Contract Management**: Digital signatures, document upload
- **Real-time Chat**: Employer-recruiter communication
- **Payment Dashboard**: Commission tracking, payout status

#### **Development Commands**
```bash
cd frontend/web

# Install dependencies
npm install

# Start development server
npm start

# Build for production
npm run build

# Run tests
npm test

# Generate components
npx generate-component
```

### **Mobile App (Flutter)**

#### **Technology Stack**
- **Flutter 3.0+** with Dart
- **State Management**: Provider + Riverpod
- **Local Storage**: Drift (SQLite) + Shared Preferences
- **Networking**: Dio HTTP Client
- **Real-time**: WebSocket Channel
- **Offline Support**: Background sync capabilities

#### **Key Features**
- **Offline-First**: Upload CVs without internet connection
- **QR Code Integration**: Share candidate profiles instantly
- **Push Notifications**: Real-time updates via Firebase
- **Camera Integration**: Scan documents and ID cards
- **GPS Location**: Location-based job matching
- **Background Jobs**: Sync data when connection available

#### **Development Commands**
```bash
cd frontend/mobile

# Install dependencies
flutter pub get

# Run on connected device
flutter run

# Run tests
flutter test

# Build APK
flutter build apk

# Build for iOS
flutter build ios
```

### **Admin Panel**

#### **Technology Stack**
- **React** with TypeScript
- **Admin Framework**: React Admin or Refine
- **Charts**: Chart.js or D3.js
- **Real-time Updates**: Server-Sent Events

#### **Key Features**
- **User Management**: Create, edit, suspend users
- **Financial Dashboard**: Transaction monitoring, payouts
- **System Monitoring**: Performance metrics, error tracking
- **Content Management**: Templates, notifications, settings
- **Analytics**: Platform-wide insights and reporting

## 🤖 AI Services

### **Service Architecture**

```
ai-services/
├── cv-parser/                   # Document Analysis
│   ├── models/                  # OCR & NLP Models
│   ├── processors/              # Text Extraction
│   └── api/                     # REST API
├── job-matcher/                 # Recommendation Engine
│   ├── algorithms/              # Matching Logic
│   ├── embeddings/              # Vector Search
│   └── scoring/                 # Similarity Scoring
├── kpi-analytics/               # Performance Prediction
│   ├── forecasting/             # Trend Analysis
│   ├── clustering/              # Performance Groups
│   └── insights/                # Actionable Insights
└── chat-bot/                    # Conversational AI
    ├── intents/                 # NLU Models
    ├── responses/               # Response Generation
    └── context/                 # Conversation Management
```

### **AI Capabilities**

#### **CV Parsing & Analysis**
- **OCR Technology**: Extract text from PDF/images
- **Entity Recognition**: Identify names, skills, experience
- **Skill Tagging**: Automatic skill categorization
- **Data Validation**: Cross-reference extracted information

#### **Smart Job Matching**
- **Content-Based Filtering**: Match based on requirements
- **Collaborative Filtering**: Learn from successful placements
- **Location Intelligence**: Consider commute and preferences
- **Real-time Scoring**: Dynamic match score updates

#### **Performance Analytics**
- **Success Prediction**: Forecast placement probability
- **Market Insights**: Industry trends and salary benchmarks
- **Recruiter Scoring**: Performance-based ranking
- **Automated Alerts**: Notify on performance changes

### **Integration Points**

```csharp
// Backend AI Service Integration
public class AIService : IAIService
{
    private readonly HttpClient _httpClient;
    private readonly string _aiBaseUrl;

    public async Task<CandidateMatchResult> MatchCandidateToJob(
        Candidate candidate, Vacancy vacancy)
    {
        var request = new MatchingRequest
        {
            CandidateProfile = await _cvParser.Parse(candidate.CVFilePath),
            JobRequirements = vacancy.Requirements,
            Location = vacancy.Location
        };

        return await _httpClient.PostAsJsonAsync<MatchingRequest, CandidateMatchResult>(
            $"{_aiBaseUrl}/match", request);
    }
}
```

## 🏗️ Infrastructure

### **Containerization Strategy**

```yaml
# docker-compose.yml
version: '3.8'
services:
  # Backend API
  api:
    build: ./backend/src/Recruitment.API
    ports:
      - "5000:80"
    depends_on:
      - database
      - redis
    environment:
      - ASPNETCORE_ENVIRONMENT=Development

  # SQL Server Database
  database:
    image: mcr.microsoft.com/sql/server:2022-latest
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=YourStrong@Passw0rd
    ports:
      - "1433:1433"
    volumes:
      - sql_data:/var/opt/mssql

  # Redis Cache
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  # AI Services
  ai-parser:
    build: ./ai-services/cv-parser
    ports:
      - "8001:80"

  # File Storage
  minio:
    image: minio/minio:latest
    command: server /data --console-address ":9001"
    ports:
      - "9000:9000"
      - "9001:9001"
    environment:
      - MINIO_ROOT_USER=minioadmin
      - MINIO_ROOT_PASSWORD=minioadmin
```

### **CI/CD Pipeline**

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production
on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run Backend Tests
        run: dotnet test backend/tests

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Build Backend
        run: dotnet publish backend/src/Recruitment.API
      - name: Build Frontend
        run: npm run build frontend/web

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Azure
        uses: azure/webapps-deploy@v2
        with:
          app-name: recruitment-platform
```

## 📊 Database

### **Schema Overview**
- **Primary Database**: SQL Server 2022
- **Total Tables**: 13 core entities + supporting tables
- **Key Features**: Comprehensive audit logging, encrypted sensitive data

### **Database Maintenance**

```sql
-- Regular maintenance tasks
EXEC sp_updatestats;                    -- Update statistics
DBCC CHECKDB;                          -- Database integrity check
EXEC sp_cleanup_audit_logs;            -- Archive old logs

-- Performance monitoring
SELECT * FROM sys.dm_exec_query_stats  -- Query performance
ORDER BY total_worker_time DESC;
```

## 🔒 Security

### **Authentication & Authorization**
- **JWT Tokens**: Access + refresh token pattern
- **Role-Based Access Control**: Employer, Recruiter, Admin, Interviewer
- **Multi-Factor Authentication**: SMS + TOTP for sensitive operations
- **Session Management**: Secure cookie handling

### **Data Protection**
- **Encryption at Rest**: AES-256 for sensitive data
- **Encryption in Transit**: TLS 1.3 for all communications
- **Banking Security**: PCI DSS compliance for payment data
- **GDPR Compliance**: Data deletion and portability features

### **Security Monitoring**
- **Audit Logging**: All user actions tracked
- **Intrusion Detection**: Failed login monitoring
- **Vulnerability Scanning**: Regular security assessments
- **Incident Response**: Automated alerting and response

## 🧪 Testing

### **Testing Strategy**

```bash
# Backend Testing
├── Unit Tests:           Business logic validation
├── Integration Tests:    Database and external services
├── E2E Tests:           Complete user workflows
└── Performance Tests:    Load testing and optimization

# Frontend Testing
├── Unit Tests:           Component logic
├── Integration Tests:    API interactions
├── E2E Tests:           User journey testing
└── Visual Tests:         UI consistency checks
```

### **Test Execution**

```bash
# Run all tests
dotnet test

# Run specific test categories
dotnet test --filter "Category=Unit"
dotnet test --filter "Category=Integration"

# Generate coverage report
dotnet test --collect:"XPlat Code Coverage"

# Frontend testing
npm test -- --coverage --watchAll=false
```

## 📦 Deployment

### **Environment Configuration**

```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=sql-server;Database=Recruitment;Trusted_Connection=True;"
  },
  "JWT": {
    "Secret": "your-super-secret-jwt-key",
    "Issuer": "recruitment-platform",
    "ExpiryMinutes": 60
  },
  "AI": {
    "BaseUrl": "http://ai-services:8000",
    "ApiKey": "your-ai-service-key"
  },
  "Payments": {
    "BankApiUrl": "https://api.bank.com",
    "ApiKey": "your-bank-api-key"
  }
}
```

### **Deployment Options**

#### **Development Environment**
```bash
# Local development setup
docker-compose -f docker-compose.dev.yml up

# Database setup
cd database/migrations
dotnet ef database update
```

#### **Production Environment**
```bash
# Azure Kubernetes Service
kubectl apply -f k8s/

# Database migration
kubectl exec deployment/database-migration -- dotnet ef database update

# Service verification
kubectl get pods,services,ingress
```

## 🔍 Monitoring

### **Application Monitoring**
- **Metrics Collection**: Prometheus metrics
- **Distributed Tracing**: Jaeger/OpenTelemetry
- **Log Aggregation**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **Error Tracking**: Sentry integration

### **Business Monitoring**
- **KPI Dashboards**: Recruiter performance metrics
- **Financial Tracking**: Commission and payment monitoring
- **System Health**: API response times, error rates
- **User Analytics**: Feature usage and engagement

### **Alerting**
- **Critical Alerts**: Payment failures, security incidents
- **Performance Alerts**: Slow response times, high error rates
- **Business Alerts**: Low conversion rates, payment delays

## 📚 API Documentation

### **OpenAPI/Swagger**
Access comprehensive API documentation at `/swagger` when running the backend API.

### **Key API Endpoints**

```bash
# Authentication
POST   /api/auth/login
POST   /api/auth/register
POST   /api/auth/refresh

# Vacancy Management
GET    /api/vacancies
POST   /api/vacancies
GET    /api/vacancies/{id}
PUT    /api/vacancies/{id}

# Application Workflow
POST   /api/vacancies/{id}/applications
GET    /api/applications/{id}
PATCH  /api/applications/{id}/status

# AI Services
POST   /api/ai/match-candidate
POST   /api/ai/parse-cv
GET    /api/ai/recommendations

# Payment Processing
POST   /api/payments/deposit
POST   /api/payments/payout
GET    /api/payments/transactions
```

## 👥 Team & Collaboration

### **Development Workflow**
1. **Feature Branching**: Create feature branches from `develop`
2. **Code Review**: All changes require peer review
3. **Automated Testing**: CI/CD runs tests on all branches
4. **Staging Deployment**: Test on staging before production

### **Communication**
- **Daily Standups**: 15-minute daily sync
- **Sprint Planning**: 2-week sprint cycles
- **Retrospectives**: Continuous improvement discussions
- **Documentation**: All decisions documented in ADR format

### **Code Quality**
- **Linting**: ESLint for JavaScript, dotnet format for C#
- **Code Coverage**: Minimum 80% coverage required
- **Security Scanning**: Automated vulnerability scanning
- **Performance Testing**: Load testing before releases

## 🚦 Getting Help

### **Common Issues**
- **Database Connection**: Check connection strings and firewall rules
- **AI Service Errors**: Verify AI service containers are running
- **Payment Failures**: Check bank API credentials and limits
- **File Upload Issues**: Verify MinIO configuration and permissions

### **Support Channels**
- **Documentation**: Check `/docs` folder for detailed guides
- **Issue Tracking**: Use GitHub Issues for bug reports
- **Discussions**: Use GitHub Discussions for questions
- **Emergency Contacts**: Critical issues contact dev leads

---

## 🎯 Success Metrics

- **Performance**: <100ms API response time, 99.9% uptime
- **Scalability**: Support 10,000+ concurrent users
- **Reliability**: Zero data loss, automated recovery
- **Security**: SOC 2 compliance, zero security incidents
- **User Experience**: >4.5 star rating, <30s feature completion

---

**Built with ❤️ for the future of recruitment**