# 📋 Backend Requirement Specifications – Airbnb Clone

This document outlines the **technical and functional specifications** for core backend features of the Airbnb Clone project. It includes API design, data validation, input/output expectations, and performance considerations.

---

## 1. 🔐 User Authentication

### ✅ Functional Requirements
- Users must be able to register, login, and logout.
- Passwords must be securely hashed and stored.
- Upon login, users should receive an authentication token (JWT).

### 📤 API Endpoints

| Method | Endpoint        | Description             |
|--------|-----------------|-------------------------|
| POST   | `/api/register/` | Register a new user     |
| POST   | `/api/login/`    | Authenticate user       |
| POST   | `/api/logout/`   | Invalidate JWT token    |

### 📥 Input Specifications

#### `POST /api/register/`
```json
{
  "email": "user@example.com",
  "password": "Password123!",
  "confirm_password": "Password123!",
  "first_name": "Chinonso",
  "last_name": "Agbo",
  "phone_number": "08105517862",
  "role": "guest"
}
```

#### Output (Success)
```
{
  "message": "User registered successfully.",
  "token": "jwt.token.here"
}
```

### ❗ Validation Rules
- Email must be unique and valid.

- Password must be at least 8 characters and include uppercase, lowercase, number, and symbol.

- Password and confirm_password must match.

### ⚙️ Performance
- Average response time ≤ 300ms.

- Password hashing via PBKDF2 or bcrypt.


## 2. 🏘️ Property Management
### ✅ Functional Requirements
- Hosts should be able to create, update, delete, and view their property listings.

- Guests can browse/search for properties.

- Properties should include images, price, location, and availability status.

### 📤 API Endpoints
| Method | Endpoint                | Description                 |
| ------ | ----------------------- | --------------------------- |
| POST   | `/api/properties/`      | Create a new listing        |
| GET    | `/api/properties/`      | List all properties         |
| GET    | `/api/properties/{id}/` | Get single property details |
| PUT    | `/api/properties/{id}/` | Update a listing            |
| DELETE | `/api/properties/{id}/` | Delete a listing            |


### 📥 Input Example
```json
{
  "title": "Luxury Apartment in Lekki",
  "description": "Spacious 2-bedroom with pool access",
  "price_per_night": 25000,
  "location": "Lekki Phase 1, Lagos",
  "images": ["url1.jpg", "url2.jpg"]
}

```

#### 📤 Output (Success)
```json
{
  "id": "p123",
  "title": "Luxury Apartment in Lekki",
  "price_per_night": 25000,
  "host_id": "u45",
  "status": "available"
}
```

### ❗ Validation Rules
- price_per_night must be a positive number.

- title is required and max 100 characters.

- location must be geocodable.

- Only authenticated hosts can create listings.

### ⚙️ Performance
- Property listing query must return ≤ 100ms for < 1,000 listings with indexing.


## 3. 📅 Booking System
### ✅ Functional Requirements
- Guests can book a property by specifying a date range.

- Overlapping bookings must be rejected.

- Hosts can view bookings on their listings.

### 📤 API Endpoints
| Method | Endpoint              | Description                 |
| ------ | --------------------- | --------------------------- |
| POST   | `/api/bookings/`      | Make a new booking          |
| GET    | `/api/bookings/`      | List all bookings (user)    |
| GET    | `/api/bookings/{id}/` | View single booking         |
| DELETE | `/api/bookings/{id}/` | Cancel booking (if allowed) |


### 📥 Input Example
```json
{
  "property_id": "p123",
  "start_date": "2025-08-01",
  "end_date": "2025-08-05"
}
```

#### 📤 Output (Success)
```json
{
  "booking_id": "b456",
  "property_id": "p123",
  "user_id": "u89",
  "status": "confirmed"
}
```

### ❗ Validation Rules
- start_date must be before end_date.

- Cannot overlap with existing bookings.

- Cannot book properties owned by the user.

### ⚙️ Performance
- Conflict-checking algorithm must validate bookings within 150ms.

- Bookings should be indexed by property and date for fast lookups.

## 📝 Notes
- All APIs are protected with JWT authentication.

- Errors follow standard JSON error format with status, message, and errors.

- Pagination and filtering are supported on list endpoints.
