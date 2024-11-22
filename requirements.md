# Requirements

## 1. User Authentication

### Functional Requirements
- **Registration**: 
  - Allow users to register as either a **guest** or a **host**.
  - Require fields: `email`, `password`, `role` (guest or host).
  - Validate email format and ensure passwords are at least 8 characters long.
- **Login**:
  - Enable login using `email` and `password`.
  - Include OAuth login options for Google and Facebook.
- **Profile Management**:
  - Allow users to update their profile details (e.g., `profilePicture`, `contactInfo`, `preferences`).

### API Endpoints
1. **POST /api/v1/auth/register**
   - **Input**: 
     ```json
     {
       "email": "user@example.com",
       "password": "securepassword",
       "role": "guest"
     }
     ```
   - **Output**: 
     ```json
     {
       "message": "User registered successfully",
       "userId": "12345"
     }
     ```
   - **Validation**: Ensure email is unique and password meets security criteria.

2. **POST /api/v1/auth/login**
   - **Input**: 
     ```json
     {
       "email": "user@example.com",
       "password": "securepassword"
     }
     ```
   - **Output**: 
     ```json
     {
       "token": "jwt-token",
       "expiresIn": "24h"
     }
     ```

### Performance Criteria
- Handle at least 100 concurrent authentication requests without a decrease in performance.

---

## 2. Property Management

### Functional Requirements
- **Create Property Listing**:
  - Allow hosts to create listings with details such as `title`, `description`, `location`, `price`, `amenities`, and `availability`.
  - Ensure all required fields are provided before saving to the database.
- **Edit/Delete Property**:
  - Hosts can update or delete their property listings.
- **Search Listings**:
  - Guests can search properties by `location`, `price range`, and `amenities`.

### API Endpoints
1. **POST /api/v1/properties**
   - **Input**: 
     ```json
     {
       "title": "Rongai Apartment",
       "description": "A beautiful apartment in the city center.",
       "location": "Rongai",
       "price": 7500,
       "amenities": ["WiFi", "Air Conditioning"],
       "availability": "2024-12-01 to 2024-12-10"
     }
     ```
   - **Output**: 
     ```json
     {
       "message": "Property listing created successfully",
       "propertyId": "98765"
     }
     ```

2. **PUT /api/v1/properties/:id**
   - **Input**: 
     ```json
     {
       "title": "Updated Rongai Apartment"
     }
     ```
   - **Output**: 
     ```json
     {
       "message": "Property updated successfully"
     }
     ```

3. **GET /api/v1/properties?location=New York&price=100-200**
   - **Output**: 
     ```json
     [
       {
         "propertyId": "98765",
         "title": "Rongai Apartment",
         "price": 7500,
         "location": "Rongai York"
       }
     ]
     ```

### Performance Criteria
- Support pagination for search results with up to 1,000 properties.

---

## 3. Booking System

### Functional Requirements
- **Create Booking**:
  - Guests can book properties for specific dates.
  - Prevent double bookings by validating availability.
- **Cancel Booking**:
  - Allow guests or hosts to cancel bookings in compliance with the cancellation policy.
- **Booking Notifications**:
  - Notify guests and hosts of booking status changes (e.g., confirmed, canceled).

### API Endpoints
1. **POST /api/v1/bookings**
   - **Input**: 
     ```json
     {
       "propertyId": "98765",
       "userId": "12345",
       "startDate": "2024-12-01",
       "endDate": "2024-12-05"
     }
     ```
   - **Output**: 
     ```json
     {
       "message": "Booking created successfully",
       "bookingId": "54321"
     }
     ```
   - **Validation**: Ensure the property is available for the specified dates.

2. **DELETE /api/v1/bookings/:id**
   - **Output**: 
     ```json
     {
       "message": "Booking canceled successfully"
     }
     ```

3. **GET /api/v1/bookings/:id**
   - **Output**: 
     ```json
     {
       "bookingId": "54321",
       "property": {
         "title": "Rongai Apartment",
         "location": "Rongai"
       },
       "dates": "2024-12-01 to 2024-12-05",
       "status": "confirmed"
     }
     ```

### Performance Criteria
- Ensure the system can process 500 bookings per minute without failures.

---

