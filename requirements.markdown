# Airbnb Clone Backend Requirement Specifications

## Objective
This document outlines the technical and functional requirements for three key backend features of the Airbnb Clone: User Authentication, Property Management, and Booking System. Each feature includes API endpoints, input/output specifications, validation rules, and performance criteria.

## 1. User Authentication

### Functional Requirements
- Allow users to register as Guests or Hosts using email/password or OAuth (Google, Facebook).
- Support login with email/password or OAuth, issuing JWT for session management.
- Enable profile updates (e.g., name, contact info, profile photo).

### Technical Requirements
- **API Endpoints**:
  - **POST /auth/register**
    - **Description**: Register a new user.
    - **Input** (JSON):
      ```json
      {
        "email": "string",
        "password": "string",
        "first_name": "string",
        "last_name": "string",
        "role": "string (guest|host)",
        "phone_number": "string (optional)"
      }
      ```
    - **Validation Rules**:
      - `email`: Valid email format, unique in Users table.
      - `password`: Min 8 characters, with uppercase, lowercase, number, special character.
      - `first_name`, `last_name`: Non-empty, max 50 characters.
      - `role`: Must be ‘guest’ or ‘host’.
      - `phone_number`: Valid phone format or null.
    - **Output** (JSON):
      ```json
      {
        "user_id": "UUID",
        "email": "string",
        "role": "string",
        "token": "JWT string"
      }
      ```
    - **Error Responses**:
      - 400: Invalid input.
      - 409: Email already exists.
  - **POST /auth/login**
    - **Description**: Authenticate a user and return a JWT.
    - **Input** (JSON):
      ```json
      {
        "email": "string",
        "password": "string"
      }
      ```
    - **Validation Rules**:
      - `email`: Must exist in Users table.
      - `password`: Must match hashed password.
    - **Output** (JSON):
      ```json
      {
        "user_id": "UUID",
        "email": "string",
        "role": "string",
        "token": "JWT string"
      }
      ```
    - **Error Responses**:
      - 401: Invalid credentials.
  - **PUT /auth/profile**
    - **Description**: Update user profile (requires JWT).
    - **Input** (JSON):
      ```json
      {
        "first_name": "string",
        "last_name": "string",
        "phone_number": "string (optional)",
        "profile_photo": "string (Cloudinary URL, optional)"
      }
      ```
    - **Validation Rules**:
      - `first_name`, `last_name`: Non-empty, max 50 characters.
      - `phone_number`: Valid format or null.
      - `profile_photo`: Valid URL or null.
    - **Output** (JSON):
      ```json
      {
        "user_id": "UUID",
        "email": "string",
        "first_name": "string",
        "last_name": "string",
        "phone_number": "string|null",
        "profile_photo": "string|null"
      }
      ```
    - **Error Responses**:
      - 401: Unauthorized.
      - 400: Invalid input.
- **OAuth Integration**: Support Google/Facebook OAuth for registration/login.
- **Database**: Store in `Users` table; use bcrypt for password hashing.
- **Security**: JWT expires in 24 hours; encrypt sensitive data.

### Performance Criteria
- Response time: < 200ms for login/register.
- Scalability: Handle 10,000 concurrent users.
- Error handling: Log authentication failures, return user-friendly messages.
- Optimization: Cache JWT validation in Redis.

## 2. Property Management

### Functional Requirements
- Allow Hosts to create, update, and delete property listings.
- Store listing details (title, description, location, price, amenities, availability).
- Restrict actions to the owning Host.

### Technical Requirements
- **API Endpoints**:
  - **POST /properties**
    - **Description**: Create a property listing (requires JWT, Host role).
    - **Input** (JSON):
      ```json
      {
        "name": "string",
        "description": "string",
        "location": "string",
        "price_per_night": "number",
        "amenities": ["string"],
        "availability": [{"start_date": "YYYY-MM-DD", "end_date": "YYYY-MM-DD"}]
      }
      ```
    - **Validation Rules**:
      - `name`: Non-empty, max 100 characters.
      - `description`: Non-empty, max 1000 characters.
      - `location`: Non-empty, max 255 characters.
      - `price_per_night`: Positive decimal.
      - `amenities`: Array of valid strings.
      - `availability`: Array of valid date ranges, `end_date > start_date`.
    - **Output** (JSON):
      ```json
      {
        "property_id": "UUID",
        "name": "string",
        "host_id": "UUID",
        "location": "string",
        "price_per_night": "number"
      }
      ```
    - **Error Responses**:
      - 401: Unauthorized.
      - 400Favorite: Invalid input.
  - **PUT /properties/{property_id}**
    - **Description**: Update a property (requires JWT, Host role, ownership).
    - **Input** (JSON): Same as POST, partial updates allowed.
    - **Validation Rules**: Same as POST.
    - **Output** (JSON): Updated property details.
    - **Error Responses**:
      - 401: Unauthorized.
      - 403: Forbidden (non-owner).
      - 404: Property not found.
  - **DELETE /properties/{property_id}**
    - **Description**: Delete a property (requires JWT, Host role, ownership).
    - **Input**: None (property_id in URL).
    - **Output** (JSON):
      ```json
      {
        "message": "Property deleted"
      }
      ```
    - **Error Responses**:
      - 401: Unauthorized.
      - 403: Forbidden.
      - 404: Property not found.
- **Database**: Store in `Properties` table; use AWS S3/Cloudinary for images.
- **Security**: Restrict to Hosts via RBAC; validate ownership.

### Performance Criteria
- Response time: < 300ms for create/update/delete.
- Scalability: Support 1,000 property creations per minute.
- Error handling: Log invalid requests, return clear messages.
- Optimization: Cache listings in Redis.

## 3. Booking System

### Functional Requirements
- Allow Guests to book properties for specific dates.
- Prevent double bookings by validating date availability.
- Support booking cancellation by Guests or Hosts.
- Track booking statuses (pending, confirmed, canceled).

### Technical Requirements
- **API Endpoints**:
  - **POST /bookings**
    - **Description**: Create a booking (requires JWT, Guest role).
    - **Input** (JSON):
      ```json
      {
        "property_id": "UUID",
        "start_date": "YYYY-MM-DD",
        "end_date": "YYYY-MM-DD",
        "payment_details": {
          "method": "string (credit_card|paypal|stripe)",
          "amount": "number"
        }
      }
      ```
    - **Validation Rules**:
      - `property_id`: Must exist in Properties.
      - `start_date`, `end_date`: Valid dates, `end_date > start_date`.
      - No overlapping bookings (check Bookings table).
      - `payment_details.method`: Valid method.
      - `amount`: Matches price_per_night × duration.
    - **Output** (JSON):
      ```json
      {
        "booking_id": "UUID",
        "property_id": "UUID",
        "user_id": "UUID",
        "start_date": "YYYY-MM-DD",
        "end_date": "YYYY-MM-DD",
        "status": "confirmed"
      }
      ```
    - **Error Responses**:
      - 401: Unauthorized.
      - 400: Invalid input or dates unavailable.
      - 404: Property not found.
  - **PUT /bookings/{booking_id}/cancel**
    - **Description**: Cancel a booking (requires JWT, Guest/Host role, ownership).
    - **Input** (JSON):
      ```json
      {
        "reason": "string (optional)"
      }
      ```
    - **Validation Rules**:
      - `booking_id`: Must exist, not already canceled.
      - User must be Guest or Host of the property.
    - **Output** (JSON):
      ```json
      {
        "booking_id": "UUID",
        "status": "canceled"
      }
      ```
    - **Error Responses**:
      - 401: Unauthorized.
      - 403: Forbidden.
      - 404: Booking not found.
- **Database**: Store in `Bookings` and `Payments` tables; use transactions.
- **Integration**: Use Stripe/PayPal for payments; SendGrid/Mailgun for notifications.
- **Security**: Validate Guest role, date availability; ensure atomic transactions.

### Performance Criteria
- Response time: < 500ms for booking creation.
- Scalability: Handle 5,000 bookings per hour.
- Error handling: Log failures, return clear messages.
- Optimization: Use indexes on `property_id`, `booking_id`.


## Conclusion
This document provides detailed requirement specifications for User Authentication, Property Management, and Booking System, including API endpoints, input/output formats, validation rules, and performance criteria. The specifications align with the Airbnb Clone’s backend requirements and support a scalable, secure system. The file is ready to be saved as `requirements.md` and committed to the root directory of the `alx-airbnb-project-documentation` repository.
