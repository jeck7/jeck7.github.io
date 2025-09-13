---
layout: default
title: "Deployment & Configuration"
description: "Complete guide for deploying and configuring the Fitness Booking Platform in production and development environments"
---

# 🚀 Deployment & Configuration

This comprehensive guide covers everything you need to know about deploying and configuring the Fitness Booking Platform, from local development setup to production deployment on Railway.

## 🌐 Live Production Environment

### Current Deployment Status

The Fitness Booking Platform is currently deployed and running on Railway:

- **Frontend**: [https://frontend-prod2-prod2.up.railway.app](https://frontend-prod2-prod2.up.railway.app)
- **Backend API**: [https://backend-prod2-prod2.up.railway.app](https://backend-prod2-prod2.up.railway.app)
- **API Documentation**: [Swagger UI](https://backend-prod2-prod2.up.railway.app/swagger-ui/index.html)

### Production Services

- **PostgreSQL Database**: `Postgres-Sum3`
- **Backend Service**: `backend-prod2`
- **Frontend Service**: `frontend-prod2`

## 🏗️ Railway Deployment

### Railway Configuration

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

### Database Configuration

#### PostgreSQL Connection Details

```bash
Host: switchyard.proxy.rlwy.net
Port: 44236
Database: railway
Username: postgres
Password: euMeabLugRVDWxQsygSmOTDAumKxfIpz
Connection URL: jdbc:postgresql://switchyard.proxy.rlwy.net:44236/railway
```

### Environment Variables

#### Backend Environment Variables

```bash
# Database Configuration
SPRING_DATASOURCE_URL=jdbc:postgresql://switchyard.proxy.rlwy.net:44236/railway
SPRING_DATASOURCE_USERNAME=postgres
SPRING_DATASOURCE_PASSWORD=euMeabLugRVDWxQsygSmOTDAumKxfIpz

# JWT Configuration
JWT_SECRET=your-secure-jwt-secret-key
JWT_EXPIRATION=900000

# CORS Configuration
CORS_ALLOWED_ORIGINS=https://frontend-prod2-prod2.up.railway.app

# Cloudinary Configuration
CLOUDINARY_CLOUD_NAME=your-cloudinary-cloud-name
CLOUDINARY_API_KEY=your-cloudinary-api-key
CLOUDINARY_API_SECRET=your-cloudinary-api-secret

# Stripe Configuration
STRIPE_SECRET_KEY=your-stripe-secret-key
STRIPE_PUBLISHABLE_KEY=your-stripe-publishable-key

# SMTP Configuration (Brevo/Sendinblue)
SPRING_MAIL_HOST=smtp-relay.brevo.com
SPRING_MAIL_PORT=587
SPRING_MAIL_USERNAME=your-brevo-email
SPRING_MAIL_PASSWORD=your-brevo-smtp-key
SPRING_MAIL_PROPERTIES_MAIL_SMTP_AUTH=true
SPRING_MAIL_PROPERTIES_MAIL_SMTP_STARTTLS_ENABLE=true

# Spring Profile
SPRING_PROFILES_ACTIVE=prod
```

#### Frontend Environment Variables

```bash
REACT_APP_API_URL=https://backend-prod2-prod2.up.railway.app
```

### Deployment Commands

#### Deploy Backend

```bash
# Deploy backend service
railway up --service backend-prod2 --path-as-root backend

# View backend logs
railway logs --service backend-prod2

# Check backend status
railway status --service backend-prod2
```

#### Deploy Frontend

```bash
# Deploy frontend service
railway up --service frontend-prod2 --path-as-root frontend

# View frontend logs
railway logs --service frontend-prod2

# Check frontend status
railway status --service frontend-prod2
```

## 🛠️ Local Development Setup

### Prerequisites

Before setting up the local development environment, ensure you have:

- **Java 17+** - Required for Spring Boot backend
- **Node.js 18+** - Required for React frontend
- **PostgreSQL** - Database server
- **Git** - Version control
- **Docker** (optional) - For containerized development

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd fitness-booking-platform
```

### Step 2: Database Setup

#### Option A: Local PostgreSQL

```bash
# Create the database
createdb fitness_booking

# Or using psql
psql -U postgres
CREATE DATABASE fitness_booking;
```

#### Option B: Docker PostgreSQL

```bash
# Run PostgreSQL in Docker
docker run --name fitness_booking_db \
  -e POSTGRES_DB=fitness_booking \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  -d postgres:15
```

### Step 3: Backend Configuration

```bash
cd backend

# Copy example configuration
cp src/main/resources/application.properties.example src/main/resources/application.properties

# Edit configuration file
nano src/main/resources/application.properties
```

#### Backend Configuration File

```properties
# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/fitness_booking
spring.datasource.username=postgres
spring.datasource.password=password
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# JWT Configuration
jwt.secret=your-local-jwt-secret
jwt.expiration=900000

# CORS Configuration
cors.allowed.origins=http://localhost:3000

# Cloudinary Configuration
cloudinary.cloud-name=your-cloudinary-cloud-name
cloudinary.api-key=your-cloudinary-api-key
cloudinary.api-secret=your-cloudinary-api-secret

# Stripe Configuration
stripe.secret-key=your-stripe-secret-key
stripe.publishable-key=your-stripe-publishable-key

# SMTP Configuration
spring.mail.host=smtp-relay.brevo.com
spring.mail.port=587
spring.mail.username=your-email
spring.mail.password=your-smtp-key
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

### Step 4: Frontend Configuration

```bash
cd frontend

# Install dependencies
npm install

# Create environment file
echo "REACT_APP_API_URL=http://localhost:8080" > .env.local
```

### Step 5: Start Development Servers

#### Backend Server

```bash
cd backend
./mvnw spring-boot:run
```

Backend will be available at: http://localhost:8080

#### Frontend Server

```bash
cd frontend
npm start
```

Frontend will be available at: http://localhost:3000

## 🐳 Docker Development

### Docker Compose Setup

The project includes a `docker-compose.yml` file for easy local development:

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: fitness_booking
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  backend:
    build: ./backend
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/fitness_booking
      - SPRING_DATASOURCE_USERNAME=postgres
      - SPRING_DATASOURCE_PASSWORD=password
    depends_on:
      - postgres

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_URL=http://localhost:8080
    depends_on:
      - backend

volumes:
  postgres_data:
```

### Running with Docker

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild and start
docker-compose up --build -d
```

## 🔧 Configuration Management

### Environment-Specific Configuration

#### Development Configuration

```properties
# application-dev.properties
spring.profiles.active=dev
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
logging.level.com.fitnessbooking=DEBUG
```

#### Production Configuration

```properties
# application-prod.properties
spring.profiles.active=prod
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=false
logging.level.com.fitnessbooking=INFO
```

### Security Configuration

#### CORS Configuration

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Value("${cors.allowed.origins}")
    private String corsOrigins;
    
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOriginPatterns(corsOrigins.split(","))
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

## 🔍 Troubleshooting

### Common Deployment Issues

#### CORS Issues

**Problem**: Frontend can't connect to backend API

**Solution**: Ensure CORS configuration includes the frontend URL:

```bash
CORS_ALLOWED_ORIGINS=https://frontend-prod2-prod2.up.railway.app
```

#### Database Connection Issues

**Problem**: Backend can't connect to database

**Solution**: Verify database credentials and connection string:

```bash
# Check database connection
psql -h switchyard.proxy.rlwy.net -p 44236 -U postgres -d railway
```

#### Build Failures

**Problem**: Railway build fails

**Solution**: Check build logs and memory limits:

```bash
# View build logs
railway logs --service backend-prod2

# Check memory usage
railway status --service backend-prod2
```

### Local Development Issues

#### Backend Won't Start

**Problem**: Spring Boot application fails to start

**Solutions**:
- Check if PostgreSQL is running
- Verify database connection settings
- Ensure Java 17+ is installed
- Check port 8080 is available

#### Frontend Can't Connect to Backend

**Problem**: React app can't reach backend API

**Solutions**:
- Verify backend is running on port 8080
- Check CORS configuration
- Verify `REACT_APP_API_URL` environment variable
- Check proxy settings in package.json

#### Database Issues

**Problem**: Database connection or query errors

**Solutions**:
- Check PostgreSQL is running
- Verify database credentials
- Check user permissions
- Verify database exists

## 📊 Monitoring & Maintenance

### Application Monitoring

#### Railway Monitoring

- **Service Status**: Check service health and uptime
- **Resource Usage**: Monitor CPU, memory, and disk usage
- **Logs**: View application and system logs
- **Metrics**: Track performance metrics

#### Database Monitoring

- **Connection Pool**: Monitor database connections
- **Query Performance**: Track slow queries
- **Storage Usage**: Monitor database size and growth
- **Backup Status**: Verify backup completion

### Maintenance Tasks

#### Regular Maintenance

- **Database Backups**: Automated daily backups
- **Log Rotation**: Manage log file sizes
- **Security Updates**: Keep dependencies updated
- **Performance Monitoring**: Track system performance

#### Emergency Procedures

- **Service Restart**: Restart failed services
- **Database Recovery**: Restore from backups
- **Rollback Procedures**: Revert to previous versions
- **Incident Response**: Handle security incidents

---

## 🔗 Related Documentation

- [Architecture & Technologies](architecture.md) - Technical architecture overview
- [Features & Functionality](features.md) - Complete feature overview
- [API Documentation](https://backend-prod2-prod2.up.railway.app/swagger-ui/index.html) - Interactive API docs
- [Back to Home](index.md)

---

*Last updated: {{ site.time | date: "%B %d, %Y" }}*
