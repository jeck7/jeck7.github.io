# Fitness Booking Platform

A modern fitness platform for booking trainers and events in gyms.

## 🏗️ Architecture

- **Backend**: Spring Boot 3.x with Java 17
- **Frontend**: React 18 with TypeScript
- **Database**: PostgreSQL
- **Authentication**: JWT access tokens + HttpOnly refresh cookie
- **UI Framework**: Material-UI (MUI) with a modern theme

## 📁 Project Structure

```
fitness-booking-platform/
├── backend/                 # Spring Boot application
│   ├── src/
│   ├── pom.xml
│   └── application.properties
├── frontend/               # React application
│   ├── src/
│   ├── package.json
│   └── public/
├── railway.toml            # Railway deployment configuration
└── docker-compose.yml      # Docker configuration
```

## 🚀 Features

### Users
- Registration and login with promo code support
- User profiles with image management
- Roles (Admin, Trainer, Client)
- Password reset functionality

### Trainers
- Trainer profiles with multiple images
- Specializations and ratings
- Work schedule management
- Image gallery with preview modals

### Gyms and Events
- Gym management with location filtering
- Event creation (indoor/outdoor events)
- Optional gym selection for outdoor events
- Event image galleries with swipe navigation

### Bookings
- Online booking with promo code discounts
- Booking calendar
- Payment integration (Stripe)
- Booking notifications

### Plan Generator
- AI-powered workout plan generation
- Personalized nutrition plans
- Plan history and progress tracking
- Plan sharing functionality

### Community Features
- Live community showcase (trainers & clients)
- Instagram integration
- Mysterious leaderboard for user engagement
- Real-time community statistics

## 🛠️ Technologies

### Backend
- Spring Boot 3.x
- Spring Security
- Spring Data JPA
- PostgreSQL
- JWT Authentication
- Cloudinary (Image Management)
- Maven

### Frontend
- React 18 with TypeScript
- Material-UI (MUI) with responsive design
- React Router for navigation
- Axios for API communication
- React Query for data fetching
- i18n for internationalization (EN/BG)
- Google Analytics 4 integration

## 📦 Recent Features & Integrations

### Cloudinary Image Management
- Switched local uploads to Cloudinary (CDN, optimization, transformations)
- Deduplication: identical images are not re-uploaded; existing URL is returned
- Optimized URLs in UI via `getOptimizedImageUrl`
- Updated components: `Layout`, `Dashboard`, `Trainers`, `Events`, `Gyms`

Backend config in `application.properties`:
- `cloudinary.cloud-name`, `cloudinary.api-key`, `cloudinary.api-secret`

### Payments (Stripe)
- Endpoints: `/api/payments/config`, `/create-payment-intent`, `/confirm/{id}`, `/my-payments`, `/{id}`
- Frontend service: `src/services/paymentService.ts`
- UI: integrated `PaymentForm` into `Bookings` for PENDING bookings; added `Payments` history page (admin-only for now)
- Stripe is initialized via `StripeProvider` and `/api/payments/config`

Admin-only (UI routes & side panel):
- `/payments` and `/subscription-plans` visible only to `ADMIN`

### Password Reset (Forgot/Reset)
- POST `/api/auth/forgot-password` → sends reset link via email
- POST `/api/auth/reset-password` → sets new password using token
- Frontend pages: `/forgot-password`, `/reset-password?token=...`
- Translations added in `locales/en.json` and `locales/bg.json`

Mail transport:
- Default mail bean included; real SMTP recommended for prod
- Brevo (Sendinblue) SMTP example in `backend/src/main/resources/application.properties.example`:
  - `spring.mail.host=smtp-relay.brevo.com`
  - `spring.mail.port=587`
  - `spring.mail.username=YOUR_BREVO_LOGIN_OR_EMAIL`
  - `spring.mail.password=YOUR_BREVO_SMTP_KEY`
  - `spring.mail.properties.mail.smtp.auth=true`
  - `spring.mail.properties.mail.smtp.starttls.enable=true`

Note: Verify sender/domain in Brevo for best deliverability

### Plan Generator & AI Features
- AI-powered workout plan generation based on user goals and preferences
- Personalized nutrition plans with dietary restrictions
- Plan history tracking and progress monitoring
- Plan sharing functionality with social media integration
- Multi-step wizard interface for plan creation

### Community & Social Features
- Live community showcase displaying real trainers and clients
- Instagram integration with embedded posts
- Mysterious leaderboard for user engagement and gamification
- Real-time community statistics and metrics
- Image galleries with swipe navigation and modal previews

### UI/UX Enhancements
- Responsive design optimized for mobile and desktop
- Modal scroll locking and touch gesture support
- Consistent image preview modals across all pages
- Improved navigation and user experience
- Internationalization support (English/Bulgarian)

### Promo Code System
- Promo code support for registrations and bookings
- Configurable discount system
- Admin-managed promotion campaigns
- Integration with payment processing

## 🔐 Roles & Access
- Payments history and Subscription plans pages: admin-only.
- Client can pay PENDING bookings via dialog in `Bookings`.

## 🧭 How to Run (Dev)
1) Backend
```
cd backend
./mvnw spring-boot:run
```
2) Frontend
```
cd frontend
npm start
```

Set `REACT_APP_API_URL` to point to your backend base URL (e.g. `http://localhost:8080`).

## 🧩 Environment Keys (Quick Reference)
Backend:
- Database (Postgres)
- JWT: `JWT_SECRET`, `JWT_EXPIRATION`
- CORS: `CORS_ALLOWED_ORIGINS`
- Cloudinary: `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`
- SMTP (Brevo): `SPRING_MAIL_*` or properties in `application.properties`

Frontend:
- `REACT_APP_API_URL`

## ✅ Post-Setup Checklist
- [ ] Add Cloudinary credentials.
- [ ] Add Brevo SMTP credentials; verify sender/domain.
- [ ] Set `REACT_APP_API_URL`.
- [ ] Test: image upload, payment flow, forgot/reset password.

## 🚀 Deployment

### Railway Deployment (Production)

The application is deployed on Railway with the following services:

#### Live URLs:
- **Frontend**: https://fitness-booking.fit
- **Backend API**: https://fitness-booking.fit/api
- **API Documentation**: https://fitness-booking.fit/api/swagger-ui/index.html

#### Railway Services:
- **PostgreSQL Database**: `Postgres-Sum3`
- **Backend Service**: `backend-prod2`
- **Frontend Service**: `frontend-prod2`

#### Database Connection (for development tools):
```
Host: switchyard.proxy.rlwy.net
Port: 44236
Database: railway
Username: postgres
Password: 
Connection URL: jdbc:postgresql://switchyard.proxy.rlwy.net:44236/railway
```

#### Railway Configuration:
The project uses `railway.toml` for service configuration:

```toml
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

#### Key Environment Variables:
- `SPRING_DATASOURCE_URL`: PostgreSQL connection string
- `JWT_SECRET`: Secret key for JWT tokens
- `CORS_ALLOWED_ORIGINS`: Allowed origins for CORS
- `REACT_APP_API_URL`: Backend API URL for frontend
- `SPRING_PROFILES_ACTIVE`: Set to "prod" for production

#### Deployment Commands:
```bash
# Deploy backend
railway up --service backend-prod2 --path-as-root backend

# Deploy frontend
railway up --service frontend-prod2 --path-as-root frontend

# View logs
railway logs --service backend-prod2
railway logs --service frontend-prod2
```

#### Troubleshooting Railway Deployment:
- **CORS Issues**: Ensure `CORS_ALLOWED_ORIGINS` includes the frontend URL
- **Database Connection**: Verify PostgreSQL service is running
- **Build Failures**: Check memory limits and build logs
- **Environment Variables**: Ensure all required variables are set

#### Important CORS Configuration:
The application uses `allowedOriginPatterns` instead of `allowedOrigins` to support credentials:
```java
// In SecurityConfig.java and WebConfig.java
.allowedOriginPatterns(corsOrigins.split(","))
.allowCredentials(true)
```

This configuration allows the frontend to send credentials (cookies) while maintaining security.

### Local Development

### Prerequisites
- Java 17+
- Node.js 18+
- PostgreSQL
- Docker (optional)

### Step 1: Clone the project

```bash
git clone <repository-url>
cd fitness-booking-platform
```

### Step 2: Database Setup

#### With PostgreSQL
```bash
# Create the database
createdb fitness_booking

# Or use psql
psql -U postgres
CREATE DATABASE fitness_booking;
```

#### With Docker
```bash
docker run --name fitness_booking_db \
  -e POSTGRES_DB=fitness_booking \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  -d postgres:15
```

### Step 3: Backend Setup

```bash
cd backend

# Copy the example configuration file
cp src/main/resources/application.properties.example src/main/resources/application.properties

# Edit application.properties according to your settings
# The following settings are particularly important:

# Set up Cloudinary (required for image uploads)
# 1. Create a free account at https://cloudinary.com/
# 2. Get your credentials from the Dashboard
# 3. Add them to application.properties or environment variables:
```

### Step 4: Cloudinary Setup

The application uses Cloudinary for image management. You need to set up a Cloudinary account:

1. **Create Cloudinary Account**: Go to [Cloudinary](https://cloudinary.com/) and create a free account
2. **Get Credentials**: From your Dashboard, copy your Cloud Name, API Key, and API Secret
3. **Configure Environment Variables**:

```bash
# Add to your environment or .env file
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret
```

Or add to `application.properties`:
```properties
cloudinary.cloud-name=your-cloud-name
cloudinary.api-key=your-api-key
cloudinary.api-secret=your-api-secret
```

**Note**: Cloudinary free tier includes 25GB storage and 25GB bandwidth per month, which is sufficient for development and small production use.

### Step 5: Backend Configuration
# - spring.datasource.url
# - spring.datasource.username
# - spring.datasource.password
# - jwt.secret

# Start the application
./mvnw spring-boot:run
```

The backend will be available at: http://localhost:8080

### Step 4: Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start the application
npm start
```

Frontend will be available at: http://localhost:3000

### Step 5: With Docker (Optional)

To run the entire application with Docker:

```bash
# From the project root directory
docker-compose up -d
```

This will start:
- PostgreSQL on port 5432
- Backend on port 8080
- Frontend on port 3000

#### Persistent database volume (recommended)

To persist PostgreSQL data across container recreations, use a named external volume:

```bash
docker volume create fitness-booking-platform_postgres_data
# docker-compose.yml is configured to use this external volume name
```

## 🔐 Configuration

Copy `application.properties.example` to `application.properties` and configure:
- Database connection
- JWT secret
- Email settings

### Authentication & session management

- The app uses short-lived access tokens and a long-lived HttpOnly refresh cookie.
- Login: `/api/auth/login` returns an access token and sets `refresh_token` cookie.
- Refresh: `/api/auth/refresh` issues a new access token based on the cookie.
- Logout: `/api/auth/logout` clears the refresh cookie.
- Frontend is configured with `axios` `withCredentials: true`; ensure you run the frontend on `http://localhost:3000` and backend on `http://localhost:8080`.
- CORS is configured to allow credentials and the `Set-Cookie` header.

## 📝 API Documentation

### Production
- **Swagger UI**: https://fitness-booking.fit/api/swagger-ui/index.html
- **OpenAPI JSON**: https://fitness-booking.fit/api/v3/api-docs

### Local Development
After starting the backend, API documentation is available at:
- Swagger UI: http://localhost:8080/api/swagger-ui/index.html
- OpenAPI JSON: http://localhost:8080/api/v3/api-docs

## 🧪 Testing the Application

### Production Testing
1. Visit https://fitness-booking.fit
2. Register a new account with promo code support
3. Log in to the system
4. Explore the different features:
  - Plan Generator for AI-powered workout plans
  - Community section with real trainers and clients
  - Event booking with optional gym selection
  - Payment processing with Stripe integration

### Local Development Testing
1. Open http://localhost:3000
2. Register a new account
3. Log in to the system
4. Explore the different features

### Key Features to Test:
- **Plan Generator**: Create personalized workout and nutrition plans
- **Community Showcase**: View real trainers and clients with image galleries
- **Event Management**: Create indoor/outdoor events with optional gym selection
- **Payment Processing**: Test booking payments with promo code discounts
- **Image Galleries**: Swipe navigation and modal previews across all pages
- **Responsive Design**: Test on mobile and desktop devices

### Notes:
- Bookings page is visible only for Clients. Trainers/Admins see recent bookings on Dashboard. Direct navigation to `/bookings` as Trainer/Admin redirects to `/dashboard`.
- Editing a Trainer now supports optional password change.
- Image uploads are handled via Cloudinary with optimization and deduplication.
- All modals support keyboard navigation and mobile swipe gestures.

## 🔧 Troubleshooting

### Backend won't start
- Check if PostgreSQL is running
- Verify settings in application.properties
- Ensure Java 17 is installed

### Frontend can't connect to backend
- Check if backend is running on port 8080
- Verify CORS settings
- Check proxy settings in package.json

### Auth refresh returns 401
- Make sure you have logged in after starting the backend so that `refresh_token` cookie is set.
- Verify the cookie exists in DevTools → Application → Cookies for `http://localhost:8080`.
- Ensure frontend requests include credentials (already configured).

### Database issues
- Check if PostgreSQL is running
- Verify database connection
- Check user permissions

### Docker issues
- Check if Docker is installed and running
- Verify that ports 3000, 8080, 5432 are available
- Use `docker-compose logs` to view errors

### Date/time and boolean serialization
- Event times must be sent in ISO format with seconds: `yyyy-MM-dd'T'HH:mm:ss`.
- Boolean `isAvailable` is consistently exposed as `isAvailable` in JSON.

## 🎨 UI/UX

- Modern and responsive design with Material-UI
- Material Design principles with custom theming
- Bilingual interface (English/Bulgarian) with i18n support
- Mobile-first design with touch gesture support
- Consistent image preview modals across all pages
- Swipe navigation for image galleries
- Modal scroll locking for better mobile experience
- Google Analytics 4 integration for user behavior tracking
- Instagram integration for social proof
- Live community showcase with real user data
