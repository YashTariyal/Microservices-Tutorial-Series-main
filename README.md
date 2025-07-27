# Microservices Tutorial Series

This project demonstrates a complete microservices architecture using Spring Boot, Spring Cloud, and various microservices patterns.

## 🏗️ Architecture Overview

The project consists of the following microservices:

### Core Infrastructure Services
- **Service Registry (Eureka Server)** - Service discovery and registration
- **Config Server** - Centralized configuration management
- **API Gateway** - Single entry point for all client requests

### Business Services
- **User Service** - User management and authentication
- **Hotel Service** - Hotel information and management
- **Rating Service** - Hotel ratings and reviews

## 📋 Prerequisites

Before running the microservices, ensure you have the following installed:

- **Java 17** or higher
- **Maven** 3.6+
- **MySQL** (optional - services can run with H2 in-memory database)

### Installing MySQL (macOS)
```bash
# Install MySQL using Homebrew
brew install mysql

# Start MySQL service
brew services start mysql
```

## 🚀 Quick Start Guide

### Step 1: Start Infrastructure Services

#### 1. Service Registry (Eureka Server)
```bash
cd ServiceRegistry/ServiceRegistry
chmod +x mvnw
./mvnw spring-boot:run
```
- **Port**: 8761
- **URL**: http://localhost:8761
- **Purpose**: Service discovery and registration

#### 2. Config Server
```bash
cd ConfigServer/ConfigServer
chmod +x mvnw
./mvnw spring-boot:run
```
- **Port**: 8085
- **URL**: http://localhost:8085
- **Purpose**: Centralized configuration management

### Step 2: Start Business Services

#### 3. User Service
```bash
cd UserService/UserService
chmod +x mvnw
./mvnw spring-boot:run
```
- **Port**: 8081
- **URL**: http://localhost:8081
- **Purpose**: User management and authentication
- **Database**: H2 in-memory (configurable for MySQL)

#### 4. Hotel Service
```bash
cd HotelService/HotelService
chmod +x mvnw
./mvnw spring-boot:run
```
- **Port**: 8082
- **URL**: http://localhost:8082
- **Purpose**: Hotel information and management

#### 5. Rating Service
```bash
cd RatingService/RatingService
chmod +x mvnw
./mvnw spring-boot:run
```
- **Port**: 8083
- **URL**: http://localhost:8083
- **Purpose**: Hotel ratings and reviews

### Step 3: Start API Gateway

#### 6. API Gateway
```bash
cd ApiGateway/ApiGateway
chmod +x mvnw
./mvnw spring-boot:run
```
- **Port**: 8084
- **URL**: http://localhost:8084
- **Purpose**: Single entry point for all client requests

## 🔧 Configuration

### Database Configuration

The services are configured to use H2 in-memory database by default. To use MySQL:

1. **Install and start MySQL**:
   ```bash
   brew install mysql
   brew services start mysql
   ```

2. **Create the database**:
   ```sql
   CREATE DATABASE microservices;
   ```

3. **Update application.yml** in each service to use MySQL:
   ```yaml
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/microservices
       username: root
       password: ""
       driver-class-name: com.mysql.cj.jdbc.Driver
     jpa:
       properties:
         hibernate:
           dialect: org.hibernate.dialect.MySQL8Dialect
   ```

### Service Discovery

All services automatically register with Eureka Server. You can view registered services at:
- **Eureka Dashboard**: http://localhost:8761

### Configuration Management

The Config Server provides centralized configuration. Configuration files are stored in a Git repository and served to all services.

## 🧪 Testing the Services

### Health Checks

Check if services are running:
```bash
# Service Registry
curl http://localhost:8761/actuator/health

# Config Server
curl http://localhost:8085/actuator/health

# User Service
curl http://localhost:8081/actuator/health

# Hotel Service
curl http://localhost:8082/actuator/health

# Rating Service
curl http://localhost:8083/actuator/health

# API Gateway
curl http://localhost:8084/actuator/health
```

### API Endpoints

#### User Service (Port 8081)
- `GET /users` - Get all users
- `GET /users/{userId}` - Get user by ID
- `POST /users` - Create new user
- `PUT /users/{userId}` - Update user
- `DELETE /users/{userId}` - Delete user

#### Hotel Service (Port 8082)
- `GET /hotels` - Get all hotels
- `GET /hotels/{hotelId}` - Get hotel by ID
- `POST /hotels` - Create new hotel
- `PUT /hotels/{hotelId}` - Update hotel
- `DELETE /hotels/{hotelId}` - Delete hotel

#### Rating Service (Port 8083)
- `GET /ratings` - Get all ratings
- `GET /ratings/{ratingId}` - Get rating by ID
- `POST /ratings` - Create new rating
- `PUT /ratings/{ratingId}` - Update rating
- `DELETE /ratings/{ratingId}` - Delete rating

#### API Gateway (Port 8084)
- All service endpoints are accessible through the gateway
- Example: `GET http://localhost:8084/users` (routes to User Service)

## 🔒 Security

The project includes OAuth2 security with Okta integration:

- **OAuth2 Resource Server**: JWT token validation
- **OAuth2 Client**: For service-to-service communication
- **Okta Integration**: External identity provider

### Security Configuration

Security is configured in each service's `application.yml`:
```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://dev-77584981.okta.com/oauth2/default
```

## 🛡️ Resilience Patterns

The project implements several resilience patterns:

### Circuit Breaker (Resilience4j)
- **Configuration**: Applied to external service calls
- **Failure Rate Threshold**: 50%
- **Wait Duration**: 6 seconds
- **Minimum Calls**: 5

### Retry Pattern
- **Max Attempts**: 3
- **Wait Duration**: 5 seconds

### Rate Limiting
- **Limit for Period**: 2 requests
- **Refresh Period**: 4 seconds
- **Timeout Duration**: 2 seconds

## 📊 Monitoring

### Actuator Endpoints

All services expose health and metrics endpoints:
- `/actuator/health` - Service health status
- `/actuator/metrics` - Application metrics
- `/actuator/info` - Application information

### Eureka Dashboard

Monitor service registration and discovery:
- **URL**: http://localhost:8761
- **Features**: Service instances, health status, metadata

## 🏃‍♂️ Running All Services

### Option 1: Manual Start (Recommended for Development)

Start services in the following order:

1. **Service Registry** (Eureka Server)
2. **Config Server**
3. **Business Services** (User, Hotel, Rating)
4. **API Gateway**

### Option 2: Using Scripts

Create a startup script to run all services:

```bash
#!/bin/bash
# start-services.sh

echo "Starting Service Registry..."
cd ServiceRegistry/ServiceRegistry && ./mvnw spring-boot:run &

echo "Waiting for Service Registry to start..."
sleep 15

echo "Starting Config Server..."
cd ../../ConfigServer/ConfigServer && ./mvnw spring-boot:run &

echo "Waiting for Config Server to start..."
sleep 15

echo "Starting User Service..."
cd ../../UserService/UserService && ./mvnw spring-boot:run &

echo "Starting Hotel Service..."
cd ../../HotelService/HotelService && ./mvnw spring-boot:run &

echo "Starting Rating Service..."
cd ../../RatingService/RatingService && ./mvnw spring-boot:run &

echo "Waiting for business services to start..."
sleep 20

echo "Starting API Gateway..."
cd ../../ApiGateway/ApiGateway && ./mvnw spring-boot:run &

echo "All services started!"
echo "Eureka Dashboard: http://localhost:8761"
echo "API Gateway: http://localhost:8084"
```

Make the script executable:
```bash
chmod +x start-services.sh
./start-services.sh
```

## 🧹 Stopping Services

To stop all services:
```bash
# Stop all Java processes
pkill -f "spring-boot:run"

# Or stop individual services
pkill -f "UserService"
pkill -f "HotelService"
pkill -f "RatingService"
pkill -f "ApiGateway"
pkill -f "ConfigServer"
pkill -f "ServiceRegistry"
```

## 🔍 Troubleshooting

### Common Issues

1. **Port Already in Use**
   ```bash
   # Check what's using the port
   lsof -i :8081
   
   # Kill the process
   kill -9 <PID>
   ```

2. **Service Registration Issues**
   - Ensure Service Registry is running first
   - Check Eureka dashboard at http://localhost:8761
   - Verify service configuration in `application.yml`

3. **Database Connection Issues**
   - For MySQL: Ensure MySQL is running and database exists
   - For H2: Check if H2 dependency is included in pom.xml

4. **Compilation Errors**
   - Update Lombok version in pom.xml if needed
   - Ensure Java 17+ is being used
   - Clean and rebuild: `./mvnw clean compile`

### Logs

Check service logs for detailed error information:
```bash
# View logs in real-time
tail -f logs/application.log

# Or check Maven output for startup errors
```

## 📁 Project Structure

```
Microservices-Tutorial-Series-main/
├── ServiceRegistry/          # Eureka Server
├── ConfigServer/            # Spring Cloud Config Server
├── UserService/             # User Management Service
├── HotelService/            # Hotel Management Service
├── RatingService/           # Rating & Review Service
├── ApiGateway/              # Spring Cloud Gateway
└── README.md               # This file
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is for educational purposes and demonstrates microservices architecture patterns.

## 🙏 Acknowledgments

- Spring Boot and Spring Cloud teams
- Netflix OSS for Eureka
- Resilience4j for circuit breaker patterns
- Okta for OAuth2 integration

---

**Happy Microservices Development! 🚀** 