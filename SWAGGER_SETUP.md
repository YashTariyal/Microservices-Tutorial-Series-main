# API Documentation Setup

This document provides information about the API documentation setup for all microservices in the project.

## Current Status

Due to Java version compatibility issues with the Maven compiler and SpringDoc OpenAPI dependencies, the Swagger documentation setup has been temporarily removed. The services are now running with clean controllers without Swagger annotations.

## Alternative API Documentation Solutions

### 1. Spring Boot Actuator (Currently Available)

All services have Spring Boot Actuator enabled which provides basic API information:

#### User Service (Port 8081)
- **Health Check**: http://localhost:8081/actuator/health
- **Info**: http://localhost:8081/actuator/info

#### Hotel Service (Port 8082)
- **Health Check**: http://localhost:8082/actuator/health
- **Info**: http://localhost:8082/actuator/info

#### Rating Service (Port 8083)
- **Health Check**: http://localhost:8083/actuator/health
- **Info**: http://localhost:8083/actuator/info

#### API Gateway (Port 8084)
- **Health Check**: http://localhost:8084/actuator/health
- **Info**: http://localhost:8084/actuator/info

### 2. Manual API Documentation

Here are the available endpoints for each service:

#### API Gateway Service (Port 8084)
- **GET** `/auth/login` - User authentication with OAuth2

#### User Service (Port 8081)
- **POST** `/users` - Create new user
- **GET** `/users/{userId}` - Get user by ID (with rate limiting)
- **GET** `/users` - Get all users

#### Hotel Service (Port 8082)
- **POST** `/hotels` - Create new hotel (Admin only)
- **GET** `/hotels/{hotelId}` - Get hotel by ID (Internal scope)
- **GET** `/hotels` - Get all hotels (Internal scope or Admin)
- **GET** `/staffs` - Get all staff members

#### Rating Service (Port 8083)
- **POST** `/ratings` - Create new rating (Admin only)
- **GET** `/ratings` - Get all ratings
- **GET** `/ratings/users/{userId}` - Get ratings by user ID
- **GET** `/ratings/hotels/{hotelId}` - Get ratings by hotel ID

## Security Information

All endpoints include security annotations and require:
- Required authorities (Admin, SCOPE_internal)
- Authentication requirements
- Access control information

## Testing the APIs

### Using curl commands:

#### User Service
```bash
# Create a user
curl -X POST http://localhost:8081/users \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","email":"john@example.com","about":"Test user"}'

# Get all users
curl http://localhost:8081/users

# Get user by ID
curl http://localhost:8081/users/{userId}
```

#### Hotel Service
```bash
# Get all hotels
curl http://localhost:8082/hotels

# Get hotel by ID
curl http://localhost:8082/hotels/{hotelId}

# Get all staff
curl http://localhost:8082/staffs
```

#### Rating Service
```bash
# Get all ratings
curl http://localhost:8083/ratings

# Get ratings by user ID
curl http://localhost:8083/ratings/users/{userId}

# Get ratings by hotel ID
curl http://localhost:8083/ratings/hotels/{hotelId}
```

## Future Improvements

To add proper Swagger documentation in the future, consider:

1. **Updating Java Version**: Ensure Java 17+ is properly configured
2. **Using SpringDoc OpenAPI 3**: Add compatible dependencies
3. **Alternative Documentation Tools**: Consider using tools like:
   - Spring REST Docs
   - Postman Collections
   - OpenAPI Generator

## Getting Started

1. **Start all microservices**
2. **Access health endpoints** to verify services are running
3. **Test APIs** using curl commands or Postman
4. **Monitor logs** for any issues

## Notes

- All services use Spring Boot 2.7.x
- Services are configured with proper security
- Rate limiting and circuit breaker patterns are implemented
- Health checks are available via Spring Boot Actuator 