---
layout: default
title: "Fitness Booking Platform"
description: "A modern full-stack fitness platform for booking trainers and events in gyms. Built with Spring Boot 3, React 18, and PostgreSQL."
---

# 🏋️ Fitness Booking Platform

A comprehensive, modern fitness platform that connects clients with trainers and gyms. Built with cutting-edge technologies and designed for scalability, this platform offers seamless booking experiences for fitness enthusiasts.

## 🚀 Live Demo

- **Frontend**: [https://frontend-prod2-prod2.up.railway.app](https://frontend-prod2-prod2.up.railway.app)
- **Backend API**: [https://backend-prod2-prod2.up.railway.app](https://backend-prod2-prod2.up.railway.app)
- **API Documentation**: [Swagger UI](https://backend-prod2-prod2.up.railway.app/swagger-ui/index.html)

## 🏗️ Architecture Overview

This is a full-stack application built with modern technologies:

- **Backend**: Spring Boot 3.x with Java 17
- **Frontend**: React 18 with TypeScript
- **Database**: PostgreSQL with JPA/Hibernate
- **Authentication**: JWT with HttpOnly refresh cookies
- **UI Framework**: Material-UI (MUI) with custom theming
- **Image Management**: Cloudinary CDN
- **Payments**: Stripe integration
- **Deployment**: Railway with Docker

## ✨ Key Features

### 👥 User Management
- **Multi-role system**: Admin, Trainer, Client
- **Secure authentication** with JWT tokens
- **Password reset** functionality
- **User profiles** with image uploads

### 🏋️‍♂️ Trainer Features
- **Detailed profiles** with specializations
- **Work schedule** management
- **Rating and review** system
- **Availability** tracking

### 🏢 Gym & Event Management
- **Gym listings** with location and amenities
- **Event creation** and management
- **Time slot booking** system
- **Real-time availability** updates

### 💳 Payment System
- **Stripe integration** for secure payments
- **Booking payments** with PENDING status
- **Payment history** tracking
- **Admin payment** management

## 🛠️ Technology Stack

### Backend Technologies
- **Spring Boot 3.x** - Modern Java framework
- **Spring Security** - Authentication and authorization
- **Spring Data JPA** - Database abstraction
- **PostgreSQL** - Relational database
- **JWT** - Token-based authentication
- **Cloudinary** - Image management and CDN
- **Stripe API** - Payment processing
- **Maven** - Dependency management

### Frontend Technologies
- **React 18** - Modern UI library
- **TypeScript** - Type-safe JavaScript
- **Material-UI (MUI)** - Component library
- **React Router** - Client-side routing
- **Axios** - HTTP client
- **React Query** - Data fetching and caching

## 📁 Project Structure

```
fitness-booking-platform/
├── backend/                 # Spring Boot application
│   ├── src/main/java/      # Java source code
│   ├── src/main/resources/ # Configuration files
│   ├── pom.xml            # Maven dependencies
│   └── application.properties
├── frontend/               # React application
│   ├── src/               # React source code
│   ├── public/            # Static assets
│   ├── package.json       # NPM dependencies
│   └── locales/           # Internationalization
├── railway.toml           # Railway deployment config
└── docker-compose.yml     # Docker configuration
```

## 🚀 Quick Start

### Prerequisites
- Java 17+
- Node.js 18+
- PostgreSQL
- Docker (optional)

### 1. Clone the Repository
```bash
git clone <repository-url>
cd fitness-booking-platform
```

### 2. Backend Setup
```bash
cd backend
cp src/main/resources/application.properties.example src/main/resources/application.properties
# Configure your database and other settings
./mvnw spring-boot:run
```

### 3. Frontend Setup
```bash
cd frontend
npm install
npm start
```

### 4. Access the Application
- Frontend: http://localhost:3000
- Backend API: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui/index.html

## 🔧 Configuration

### Environment Variables
- **Database**: PostgreSQL connection string
- **JWT**: Secret key and expiration settings
- **Cloudinary**: Image management credentials
- **Stripe**: Payment processing keys
- **SMTP**: Email service configuration

### Key Configuration Files
- `application.properties` - Backend configuration
- `railway.toml` - Deployment configuration
- `docker-compose.yml` - Local development setup

## 📚 Documentation

- [Architecture & Technologies](architecture.md) - Detailed technical overview
- [Features & Functionality](features.md) - Complete feature list
- [Deployment Guide](deployment.md) - Production deployment instructions
- [API Documentation](https://backend-prod2-prod2.up.railway.app/swagger-ui/index.html) - Interactive API docs

## 🎯 Development Highlights

### Modern Development Practices
- **TypeScript** for type safety
- **JWT authentication** with refresh tokens
- **RESTful API** design
- **Responsive design** for all devices
- **Internationalization** support (EN/BG)

### Security Features
- **HttpOnly cookies** for refresh tokens
- **CORS** configuration for cross-origin requests
- **Input validation** and sanitization
- **Role-based access control**

### Performance Optimizations
- **Cloudinary CDN** for image delivery
- **React Query** for efficient data fetching
- **Lazy loading** for better performance
- **Database indexing** for fast queries

## 🤝 Contributing

This project is open source and welcomes contributions! Please see our [Contributing Guidelines](about.md#contributing) for details.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🔗 Quick Links

- [View Live Demo](https://frontend-prod2-prod2.up.railway.app)
- [API Documentation](https://backend-prod2-prod2.up.railway.app/swagger-ui/index.html)
- [GitHub Repository](https://github.com/jeck7/fitness-booking-platform)
- [About the Project](about.md)

---

*Last updated: {{ site.time | date: "%B %d, %Y" }}*
