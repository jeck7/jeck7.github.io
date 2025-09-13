---
layout: default
title: "Architecture & Technologies"
description: "Detailed technical overview of the Fitness Booking Platform architecture, technologies, and development practices"
---

# 🏗️ Architecture & Technologies

This page provides a comprehensive technical overview of the Fitness Booking Platform, covering the architecture, technology stack, and development practices used in building this modern full-stack application.

## 🎯 System Architecture

The Fitness Booking Platform follows a modern microservices-oriented architecture with clear separation of concerns:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React Frontend │    │  Spring Boot    │    │   PostgreSQL    │
│   (Port 3000)   │◄──►│   Backend API   │◄──►│   Database      │
│                 │    │   (Port 8080)   │    │   (Port 5432)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Cloudinary    │    │   Stripe API    │    │   SMTP Service  │
│   (Image CDN)   │    │   (Payments)    │    │   (Email)       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🔧 Backend Architecture

### Spring Boot 3.x Application

The backend is built using Spring Boot 3.x with Java 17, following modern Spring practices:

#### Core Components

```java
// Main application structure
com.fitnessbooking
├── config/          # Configuration classes
├── controller/      # REST controllers
├── service/         # Business logic
├── repository/      # Data access layer
├── entity/          # JPA entities
├── dto/            # Data transfer objects
├── security/       # Security configuration
└── exception/      # Exception handling
```

#### Key Technologies

- **Spring Boot 3.x** - Main framework
- **Spring Security** - Authentication & authorization
- **Spring Data JPA** - Database abstraction
- **Spring Web** - REST API development
- **Spring Mail** - Email functionality
- **Maven** - Dependency management

### Database Design

#### PostgreSQL Schema

```sql
-- Core entities
Users (id, email, password, role, profile_image, created_at)
Trainers (id, user_id, specializations, rating, bio)
Gyms (id, name, location, amenities, images)
Events (id, gym_id, trainer_id, title, description, start_time, end_time, max_participants)
Bookings (id, user_id, event_id, status, payment_status, created_at)
Payments (id, booking_id, stripe_payment_intent_id, amount, status)
```

#### Key Features

- **JPA/Hibernate** for ORM
- **Database migrations** with Flyway
- **Connection pooling** with HikariCP
- **Query optimization** with proper indexing
- **Transaction management** with Spring

### Authentication & Security

#### JWT Implementation

```java
// JWT Configuration
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public JwtAuthenticationFilter jwtAuthenticationFilter() {
        return new JwtAuthenticationFilter();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

#### Security Features

- **JWT Access Tokens** (short-lived, 15 minutes)
- **HttpOnly Refresh Cookies** (long-lived, 7 days)
- **Role-based Access Control** (ADMIN, TRAINER, CLIENT)
- **CORS Configuration** for cross-origin requests
- **Password Encryption** with BCrypt
- **Input Validation** and sanitization

## 🎨 Frontend Architecture

### React 18 with TypeScript

The frontend is built using React 18 with TypeScript for type safety and modern development practices:

#### Project Structure

```
src/
├── components/     # Reusable UI components
├── pages/         # Page components
├── services/      # API service layer
├── hooks/         # Custom React hooks
├── context/       # React context providers
├── types/         # TypeScript type definitions
├── utils/         # Utility functions
├── locales/       # Internationalization
└── styles/        # Styling files
```

#### Key Technologies

- **React 18** - UI library with concurrent features
- **TypeScript** - Type-safe JavaScript
- **Material-UI (MUI)** - Component library
- **React Router** - Client-side routing
- **Axios** - HTTP client with interceptors
- **React Query** - Data fetching and caching
- **i18n** - Internationalization (English/Bulgarian)
- **Google Analytics 4** - User behavior tracking
- **React Hook Form** - Form management

### State Management

#### Context API + React Query

```typescript
// Authentication Context
const AuthContext = createContext<AuthContextType | undefined>(undefined);

// React Query for data fetching
const { data: user, isLoading } = useQuery({
  queryKey: ['user'],
  queryFn: fetchUser,
  staleTime: 5 * 60 * 1000, // 5 minutes
});
```

#### State Management Features

- **React Context** for global state (auth, theme)
- **React Query** for server state management
- **Local state** with useState and useReducer
- **Form state** with React Hook Form
- **URL state** with React Router

## 🔌 API Design

### RESTful API Principles

The API follows RESTful design principles with clear resource-based URLs:

#### Endpoint Structure

```
/api/auth/
├── POST /login          # User authentication
├── POST /register       # User registration
├── POST /refresh        # Token refresh
├── POST /logout         # User logout
├── POST /forgot-password # Password reset request
└── POST /reset-password  # Password reset confirmation

/api/users/
├── GET    /profile      # Get user profile
├── PUT    /profile      # Update user profile
└── POST   /upload       # Upload profile image

/api/trainers/
├── GET    /             # List all trainers
├── GET    /{id}         # Get trainer details
├── POST   /             # Create trainer (admin)
├── PUT    /{id}         # Update trainer
└── DELETE /{id}         # Delete trainer (admin)

/api/events/
├── GET    /             # List all events
├── GET    /{id}         # Get event details
├── POST   /             # Create event
├── PUT    /{id}         # Update event
└── DELETE /{id}         # Delete event

/api/bookings/
├── GET    /             # List user bookings
├── POST   /             # Create booking
├── PUT    /{id}         # Update booking
└── DELETE /{id}         # Cancel booking

/api/payments/
├── POST   /create-payment-intent # Create Stripe payment
├── POST   /confirm/{id}          # Confirm payment
├── GET    /my-payments           # Get payment history
└── GET    /{id}                  # Get payment details
```

### API Features

- **OpenAPI 3.0** documentation with Swagger UI
- **Request/Response validation** with Bean Validation
- **Error handling** with custom exception classes
- **Rate limiting** and security headers
- **CORS configuration** for cross-origin requests

## 🤖 AI & Advanced Features Architecture

### AI-Powered Plan Generator

#### Workout Plan Generation
- **AI Integration** - Machine learning algorithms for personalized workout plans
- **Goal-Based Planning** - Plans tailored to specific fitness objectives
- **Difficulty Adaptation** - Dynamic plan adjustment based on user progress
- **Multi-step Wizard** - User-friendly interface for plan customization

#### Nutrition Planning System
- **Dietary Restrictions** - Support for various dietary needs and preferences
- **Macro Tracking** - Detailed macronutrient breakdown and monitoring
- **Recipe Integration** - Access to healthy recipes and meal suggestions
- **Plan Export** - Export plans in various formats (PDF, image, etc.)

### Community & Social Features

#### Live Community Showcase
- **Real User Display** - Showcase of actual trainers and clients
- **Image Galleries** - Swipe navigation with modal previews
- **Instagram Integration** - Embedded social media content
- **Real-time Statistics** - Live community metrics and engagement data

#### Gamification System
- **Leaderboard** - Engaging ranking system for user motivation
- **Achievement System** - Badges and rewards for accomplishments
- **Social Sharing** - Easy sharing of achievements and progress
- **Community Challenges** - Regular fitness challenges and competitions

## 🚀 Deployment Architecture

### Railway Deployment

The application is deployed on Railway with the following setup:

#### Services

- **PostgreSQL Database** - `Postgres-Sum3`
- **Backend Service** - `backend-prod2`
- **Frontend Service** - `frontend-prod2`

#### Configuration

```toml
# railway.toml
[project]
name = "fitness-booking-platform"

[[services]]
name = "backend-prod2"
root = "backend"
build = "./mvnw -DskipTests package"
start = "java -Dserver.port=$PORT -jar target/*.jar"

[[services]]
name = "frontend-prod2"
root = "frontend"
build = "npm ci && npm run build"
start = "npx serve -s build -l $PORT"
```

### Environment Configuration

#### Backend Environment Variables

```bash
# Database
SPRING_DATASOURCE_URL=jdbc:postgresql://switchyard.proxy.rlwy.net:44236/railway
SPRING_DATASOURCE_USERNAME=postgres
SPRING_DATASOURCE_PASSWORD=euMeabLugRVDWxQsygSmOTDAumKxfIpz

# JWT
JWT_SECRET=your-jwt-secret
JWT_EXPIRATION=900000

# CORS
CORS_ALLOWED_ORIGINS=https://frontend-prod2-prod2.up.railway.app

# Cloudinary
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret

# Stripe
STRIPE_SECRET_KEY=your-stripe-secret-key
STRIPE_PUBLISHABLE_KEY=your-stripe-publishable-key

# SMTP
SPRING_MAIL_HOST=smtp-relay.brevo.com
SPRING_MAIL_PORT=587
SPRING_MAIL_USERNAME=your-email
SPRING_MAIL_PASSWORD=your-smtp-key
```

#### Frontend Environment Variables

```bash
REACT_APP_API_URL=https://backend-prod2-prod2.up.railway.app
```

## 🔧 Development Practices

### Code Quality

- **TypeScript** for type safety
- **ESLint** and **Prettier** for code formatting
- **Husky** for pre-commit hooks
- **Jest** and **React Testing Library** for testing
- **SonarQube** for code quality analysis

### CI/CD Pipeline

- **GitHub Actions** for automated testing
- **Railway** for automated deployment
- **Docker** for containerization
- **Environment-specific** configurations

### Performance Optimizations

- **Cloudinary CDN** for image delivery
- **React Query** for efficient data fetching
- **Code splitting** with React.lazy()
- **Image optimization** and lazy loading
- **Database indexing** for fast queries

## 📊 Monitoring & Logging

### Application Monitoring

- **Railway logs** for application monitoring
- **Database monitoring** with PostgreSQL metrics
- **Error tracking** with custom exception handling
- **Performance metrics** with Spring Boot Actuator

### Security Monitoring

- **JWT token** validation and expiration
- **Rate limiting** for API endpoints
- **CORS** monitoring and validation
- **Input validation** and sanitization

---

## 🔗 Related Documentation

- [Features & Functionality](features.md) - Complete feature overview
- [Deployment Guide](deployment.md) - Production deployment instructions
- [API Documentation](https://backend-prod2-prod2.up.railway.app/swagger-ui/index.html) - Interactive API docs
- [Back to Home](index.md)

---

*Last updated: {{ site.time | date: "%B %d, %Y" }}*
