Below is an example of a very detailed README.md file for your Real Estate API project. This documentation covers all endpoints, input parameters, sample requests/responses, and usage scenarios. You can adjust it as needed before publishing on GitHub.

---

```markdown
# Real Estate API Project Documentation

## Introduction

This project provides a comprehensive API for managing real estate properties along with additional features such as house reservations, user comments & ratings, blog posts, user management, payment processing, advanced geo-search, real-time notifications, chat functionality, and social integration. The API is implemented using **Node.js**, **Express.js**, and **TypeScript**. Sequelize is used as the ORM for database interactions.

## Table of Contents

- [Technical Overview](#technical-overview)
- [Authentication & Authorization](#authentication--authorization)
- [Endpoints](#endpoints)
  - [Properties (Houses)](#1-properties-houses)
  - [Users](#2-users)
  - [Bookings](#3-bookings)
  - [Comments & Ratings](#4-comments--ratings)
  - [Payment & Reservations](#5-payment--reservations)
  - [Blogs](#6-blogs)
  - [Categories](#7-categories)
  - [Favorites](#8-favorites)
  - [Locations](#9-locations)
  - [Social Media Links](#10-social-media-links)
  - [Social Authentication](#11-social-authentication)
  - [Social Sharing](#12-social-sharing)
  - [Advanced Geo-Search](#13-advanced-geo-search)
  - [Real-time Notifications & Chat](#14-real-time-notifications--chat)
  - [Advanced Reporting (Dashboard)](#15-advanced-reporting-dashboard)
  - [User Profile Management](#16-user-profile-management)
- [Setup & Installation](#setup--installation)
- [Usage Examples](#usage-examples)
- [Future Enhancements](#future-enhancements)

---

## Technical Overview

- **Backend Framework:** Node.js with Express.js  
- **Language:** TypeScript  
- **ORM:** Sequelize  
- **Real-time Communication:** Socket.IO  
- **Authentication:** JWT with refresh tokens and Passport.js for social login  
- **Database:** PostgreSQL (recommended with PostGIS for geo-search)  
- **Other Technologies:** express-validator for input validation

---

## Authentication & Authorization

- **JWT Authentication:** The API uses JWT tokens for user authentication. An access token (short-lived) and a refresh token (long-lived) are issued upon successful login.
- **Social Authentication:** Users can also authenticate via social networks (Facebook, Google, LinkedIn) using Passport.js strategies.
- **Role-Based Access:** Certain endpoints (e.g., admin dashboard, advanced reporting) are restricted to users with the `admin` role. Middleware (`authenticateToken` and `authorizeRoles`) ensures that only authorized users can access these endpoints.

---

## Endpoints

Below is a detailed description of each endpoint category, including the HTTP method, URL, required parameters, sample requests/responses, and usage scenarios.

---

### 1. Properties (Houses)

#### **GET /api/houses**

- **Description:**  
  Retrieve all houses with filtering, pagination, and sorting.
- **Query Parameters:**
  - `page` (optional, default: "1") – Page number for pagination.
  - `limit` (optional, default: "10") – Number of houses per page.
  - `sort` (optional) – Field to sort by (e.g., "price", "rate").
  - `order` (optional, default: "ASC") – Sorting order ("ASC" or "DESC").
  - Additional filters: `title`, `address`, `rate`, `price`, `capacity`, `transaction_type`.
- **Sample Request:**
  ```
  GET /api/houses?page=1&limit=10&sort=price&order=DESC&transaction_type=rental
  ```
- **Sample Response:**
  ```json
  [
    {
      "id": 1,
      "title": "Modern Apartment",
      "address": "123 Main St",
      "rate": 4.5,
      "price": 1200,
      "capacity": 4,
      "transaction_type": "rental",
      ...
    },
    ...
  ]
  ```

#### **GET /api/houses/:id**

- **Description:**  
  Retrieve detailed information about a specific house by its ID.
- **URL Parameter:**
  - `id` – ID of the house.
- **Sample Request:**
  ```
  GET /api/houses/1
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "Modern Apartment",
    "address": "123 Main St",
    "rate": 4.5,
    "price": 1200,
    "capacity": 4,
    "transaction_type": "rental",
    "createdAt": "2023-01-01T00:00:00.000Z",
    ...
  }
  ```

#### **POST /api/houses**

- **Description:**  
  Create a new house.
- **Request Body (JSON):**
  ```json
  {
    "title": "Modern Apartment",
    "address": "123 Main St",
    "rate": 4.5,
    "price": 1200,
    "capacity": 4,
    "transaction_type": "rental",
    "photos": ["photo1.jpg", "photo2.jpg"],
    ...
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "Modern Apartment",
    "address": "123 Main St",
    "rate": 4.5,
    "price": 1200,
    "capacity": 4,
    "transaction_type": "rental",
    "createdAt": "2023-01-01T00:00:00.000Z",
    ...
  }
  ```

#### **PUT /api/houses/:id**

- **Description:**  
  Update house information.
- **URL Parameter:**  
  - `id` – ID of the house.
- **Request Body (JSON):**
  ```json
  {
    "price": 1300,
    "capacity": 5
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "Modern Apartment",
    "address": "123 Main St",
    "rate": 4.5,
    "price": 1300,
    "capacity": 5,
    "transaction_type": "rental",
    "updatedAt": "2023-02-01T00:00:00.000Z",
    ...
  }
  ```

#### **DELETE /api/houses/:id**

- **Description:**  
  Delete a house.
- **URL Parameter:**  
  - `id` – ID of the house.
- **Sample Response:**
  ```json
  { "message": "House deleted successfully" }
  ```

---

### 2. Users

#### **POST /api/users/register**

- **Description:**  
  Register a new user.
- **Request Body (JSON):**
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword",
    "role": "buyer",
    "name": "John Doe"
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "name": "John Doe",
    "createdAt": "2023-01-01T00:00:00.000Z",
    ...
  }
  ```

#### **POST /api/users/login**

- **Description:**  
  User login. Returns a JWT token on successful authentication.
- **Request Body (JSON):**
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword"
  }
  ```
- **Sample Response:**
  ```json
  { "token": "jwt_token_here" }
  ```

#### **GET /api/users**

- **Description:**  
  Retrieve all users with filtering support.
- **Query Parameters:**  
  - Filtering by `role`, `membership_date`, etc.
- **Sample Request:**
  ```
  GET /api/users?role=buyer&page=1&limit=10
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "email": "user@example.com", "role": "buyer", ... },
    ...
  ]
  ```

#### **GET /api/users/:id**

- **Description:**  
  Retrieve details of a specific user.
- **URL Parameter:**  
  - `id` – User ID.
- **Sample Request:**
  ```
  GET /api/users/1
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "name": "John Doe",
    "profile": {
      "address": "123 Main St",
      "phone": "09123456789",
      "buyingHistory": [...],
      "sellingHistory": [...]
    },
    ...
  }
  ```

#### **PUT /api/users/:id**

- **Description:**  
  Update a user's information. If the password is provided, it is re-hashed.
- **Request Body (JSON):**
  ```json
  {
    "name": "John Doe Updated",
    "password": "newSecurePassword"
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe Updated",
    ...
  }
  ```

#### **DELETE /api/users/:id**

- **Description:**  
  Delete a user by their ID.
- **Sample Response:**
  ```json
  { "message": "User deleted successfully" }
  ```

---

### 3. Bookings

#### **GET /api/bookings**

- **Description:**  
  Retrieve all bookings with filtering, pagination, and sorting.
- **Sample Request:**
  ```
  GET /api/bookings?page=1&limit=10&house_id=2
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], ... },
    ...
  ]
  ```

#### **GET /api/bookings/:id**

- **Description:**  
  Retrieve details of a specific booking.
- **Sample Request:**
  ```
  GET /api/bookings/1
  ```
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], ... }
  ```

#### **POST /api/bookings**

- **Description:**  
  Create a new booking.
- **Request Body (JSON):**
  ```json
  {
    "house_id": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": { "name": "Alice", "contact": "09123456789" }
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "house_id": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": { "name": "Alice", "contact": "09123456789" },
    ...
  }
  ```

#### **PUT /api/bookings/:id**

- **Description:**  
  Update booking information.
- **Request Body (JSON):**
  ```json
  {
    "reservedDates": ["2023-05-02", "2023-05-12"]
  }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-02", "2023-05-12"], ... }
  ```

#### **DELETE /api/bookings/:id**

- **Description:**  
  Delete a booking by its ID.
- **Sample Response:**
  ```json
  { "message": "Booking deleted successfully" }
  ```

---

### 4. Comments & Ratings

#### **GET /api/comments**

- **Description:**  
  Retrieve all comments with filtering support based on house_id, user_id, or rating.
- **Sample Request:**
  ```
  GET /api/comments?house_id=1&page=1&limit=10
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "house_id": 1, "user_id": 2, "rating": 4.5, "comment": "Great property!", ... },
    ...
  ]
  ```

#### **GET /api/comments/:id**

- **Description:**  
  Retrieve details of a specific comment.
- **Sample Request:**
  ```
  GET /api/comments/1
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "house_id": 1,
    "user_id": 2,
    "rating": 4.5,
    "comment": "Great property!",
    ...
  }
  ```

#### **POST /api/comments**

- **Description:**  
  Create a new comment and rating.
- **Request Body (JSON):**
  ```json
  {
    "house_id": 1,
    "user_id": 2,
    "rating": 4.5,
    "comment": "Great property!"
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "house_id": 1,
    "user_id": 2,
    "rating": 4.5,
    "comment": "Great property!",
    "createdAt": "2023-03-01T00:00:00.000Z",
    ...
  }
  ```

#### **PUT /api/comments/:id**

- **Description:**  
  Update an existing comment or rating.
- **Request Body (JSON):**
  ```json
  {
    "rating": 4.8,
    "comment": "Updated comment: excellent property!"
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "house_id": 1,
    "user_id": 2,
    "rating": 4.8,
    "comment": "Updated comment: excellent property!",
    "updatedAt": "2023-03-05T00:00:00.000Z",
    ...
  }
  ```

#### **DELETE /api/comments/:id**

- **Description:**  
  Delete a comment by its ID.
- **Sample Response:**
  ```json
  { "message": "Comment deleted successfully" }
  ```

---

### 5. Payment & Reservations

#### **POST /api/payments/checkout**

- **Description:**  
  Initiate a payment for a booking.
- **Request Body (JSON):**
  ```json
  {
    "bookingId": 1,
    "amount": 1200
  }
  ```
- **Sample Response:**
  ```json
  {
    "authority": "FAKE_AUTH_CODE_12345",
    "paymentUrl": "https://payment-gateway.com/pay/FAKE_AUTH_CODE_12345"
  }
  ```

#### **GET /api/payments/status/:transactionId**

- **Description:**  
  Check the payment status of a transaction.
- **URL Parameter:**  
  - `transactionId` – The ID of the transaction.
- **Sample Response:**
  ```json
  { "status": "paid" }
  ```

#### **POST /api/payments/verify**

- **Description:**  
  Verify a payment after a successful transaction.
- **Request Body (JSON):**
  ```json
  {
    "authority": "FAKE_AUTH_CODE_12345",
    "status": "OK",
    "bookingId": 1
  }
  ```
- **Sample Response (if verified successfully):**
  ```json
  { "message": "Payment successful and booking updated" }
  ```

---

### 6. Blogs

#### **GET /api/blogs**

- **Description:**  
  Retrieve all blog posts with filtering support based on title, author_id, or category_id.
- **Sample Request:**
  ```
  GET /api/blogs?page=1&limit=10
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "title": "Real Estate Trends", "author_id": 2, "category_id": 3, ... },
    ...
  ]
  ```

#### **GET /api/blogs/:id**

- **Description:**  
  Retrieve details of a specific blog post.
- **Sample Request:**
  ```
  GET /api/blogs/1
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "Real Estate Trends",
    "content": "Detailed blog content here...",
    "author_id": 2,
    "category_id": 3,
    ...
  }
  ```

#### **POST /api/blogs**

- **Description:**  
  Create a new blog post.
- **Request Body (JSON):**
  ```json
  {
    "title": "New Trends in Real Estate",
    "content": "Blog content goes here...",
    "author_id": 2,
    "category_id": 3
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "New Trends in Real Estate",
    "content": "Blog content goes here...",
    "author_id": 2,
    "category_id": 3,
    "createdAt": "2023-03-01T00:00:00.000Z",
    ...
  }
  ```

#### **PUT /api/blogs/:id**

- **Description:**  
  Update a blog post.
- **Request Body (JSON):**
  ```json
  {
    "title": "Updated Real Estate Trends",
    "content": "Updated blog content..."
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "Updated Real Estate Trends",
    "content": "Updated blog content...",
    "updatedAt": "2023-03-05T00:00:00.000Z",
    ...
  }
  ```

#### **DELETE /api/blogs/:id**

- **Description:**  
  Delete a blog post.
- **Sample Response:**
  ```json
  { "message": "Blog post deleted successfully" }
  ```

---

### 7. Categories

#### **GET /api/categories**

- **Description:**  
  Retrieve all categories.
- **Sample Response:**
  ```json
  [
    { "id": 1, "name": "Residential" },
    { "id": 2, "name": "Commercial" },
    ...
  ]
  ```

#### **GET /api/categories/:id**

- **Description:**  
  Retrieve details of a specific category.
- **Sample Request:**
  ```
  GET /api/categories/1
  ```
- **Sample Response:**
  ```json
  { "id": 1, "name": "Residential" }
  ```

#### **POST /api/categories**

- **Description:**  
  Create a new category.
- **Request Body (JSON):**
  ```json
  { "name": "Luxury" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "name": "Luxury" }
  ```

#### **PUT /api/categories/:id**

- **Description:**  
  Update a category.
- **Request Body (JSON):**
  ```json
  { "name": "Updated Category Name" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "name": "Updated Category Name" }
  ```

#### **DELETE /api/categories/:id**

- **Description:**  
  Delete a category.
- **Sample Response:**
  ```json
  { "message": "Category deleted successfully" }
  ```

---

### 8. Favorites

#### **POST /api/favorites/add**

- **Description:**  
  Add a house to a user's favorites.
- **Request Body (JSON):**
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "user_id": 1, "house_id": 2 }
  ```

#### **DELETE /api/favorites/remove**

- **Description:**  
  Remove a house from a user's favorites.
- **Request Body (JSON):**
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Sample Response:**
  ```json
  { "message": "Favorite removed successfully" }
  ```

#### **GET /api/favorites/:user_id**

- **Description:**  
  Retrieve all favorite houses for a specific user with pagination.
- **Sample Request:**
  ```
  GET /api/favorites/1?page=1&limit=10
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "user_id": 1, "house_id": 2, ... },
    ...
  ]
  ```

---

### 9. Locations

#### **GET /api/locations**

- **Description:**  
  Retrieve all locations with filtering support based on area_name, lat, or lng.
- **Sample Request:**
  ```
  GET /api/locations?page=1&limit=10
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "area_name": "Downtown", "lat": 35.6892, "lng": 51.3890 },
    ...
  ]
  ```

#### **GET /api/locations/:id**

- **Description:**  
  Retrieve details of a specific location.
- **Sample Request:**
  ```
  GET /api/locations/1
  ```
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "Downtown", "lat": 35.6892, "lng": 51.3890 }
  ```

#### **POST /api/locations**

- **Description:**  
  Create a new location.
- **Request Body (JSON):**
  ```json
  { "area_name": "Uptown", "lat": 35.7000, "lng": 51.4000 }
  ```
- **Sample Response:**
  ```json
  { "id": 2, "area_name": "Uptown", "lat": 35.7000, "lng": 51.4000 }
  ```

#### **PUT /api/locations/:id**

- **Description:**  
  Update a location.
- **Request Body (JSON):**
  ```json
  { "area_name": "Updated Area" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "Updated Area", "lat": 35.6892, "lng": 51.3890 }
  ```

#### **DELETE /api/locations/:id**

- **Description:**  
  Delete a location.
- **Sample Response:**
  ```json
  { "message": "Location deleted successfully" }
  ```

---

### 10. Social Media Links

#### **GET /api/social-media-links**

- **Description:**  
  Retrieve all social media links with filtering support based on platform or URL.
- **Sample Request:**
  ```
  GET /api/social-media-links?page=1&limit=10
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "platform": "Facebook", "url": "https://facebook.com/realestate" },
    ...
  ]
  ```

#### **GET /api/social-media-links/:id**

- **Description:**  
  Retrieve details of a specific social media link.
- **Sample Request:**
  ```
  GET /api/social-media-links/1
  ```
- **Sample Response:**
  ```json
  { "id": 1, "platform": "Facebook", "url": "https://facebook.com/realestate" }
  ```

#### **POST /api/social-media-links**

- **Description:**  
  Create a new social media link.
- **Request Body (JSON):**
  ```json
  { "platform": "Twitter", "url": "https://twitter.com/realestate" }
  ```
- **Sample Response:**
  ```json
  { "id": 2, "platform": "Twitter", "url": "https://twitter.com/realestate" }
  ```

#### **PUT /api/social-media-links/:id**

- **Description:**  
  Update a social media link.
- **Request Body (JSON):**
  ```json
  { "url": "https://twitter.com/newrealestate" }
  ```
- **Sample Response:**
  ```json
  { "id": 2, "platform": "Twitter", "url": "https://twitter.com/newrealestate" }
  ```

#### **DELETE /api/social-media-links/:id**

- **Description:**  
  Delete a social media link.
- **Sample Response:**
  ```json
  { "message": "Social media link deleted successfully" }
  ```

---

### 11. Social Authentication

#### **POST /api/auth/social/{provider}**  
*(Provider can be facebook, google, or linkedin)*

- **Description:**  
  Authenticate using a social network. On success, the user is either logged in or registered, and a JWT token is issued.
- **Usage:**  
  This is handled by Passport.js. The routes for each provider are defined in `routes/socialAuthRoutes.ts` and include callback URLs.
- **Example:**  
  - `GET /api/auth/social/facebook` – Redirects to Facebook for authentication.
  - `GET /api/auth/social/facebook/callback` – Callback endpoint after authentication.

---

### 12. Social Sharing

#### **GET /api/share/property**

- **Description:**  
  Generate a shareable URL for a property.
- **Query Parameters:**  
  - `propertyId` – The ID of the property to share.
- **Sample Request:**
  ```
  GET /api/share/property?propertyId=1
  ```
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/properties/1" }
  ```

#### **GET /api/share/comment**

- **Description:**  
  Generate a shareable URL for a comment.
- **Query Parameters:**  
  - `commentId` – The ID of the comment to share.
- **Sample Request:**
  ```
  GET /api/share/comment?commentId=1
  ```
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/comments/1" }
  ```

---

### 13. Advanced Geo-Search

#### **GET /api/houses/geo-search**

- **Description:**  
  Retrieve houses within a specified geographical radius. Uses PostGIS functions for geo-search.
- **Query Parameters:**  
  - `lat` – Latitude (required)
  - `lng` – Longitude (required)
  - `radius` – Search radius in meters (required)
- **Sample Request:**
  ```
  GET /api/houses/geo-search?lat=35.6892&lng=51.3890&radius=5000
  ```
- **Sample Response:**
  ```json
  [
    { "id": 3, "title": "House in Downtown", ... },
    ...
  ]
  ```

---

### 14. Real-time Notifications & Chat

#### **Notifications**

##### **POST /api/notifications/send**

- **Description:**  
  Send a real-time notification to a specific room (e.g., user ID) via Socket.IO.
- **Request Body (JSON):**
  ```json
  {
    "room": "user_1",
    "notification": "Your booking has been confirmed!"
  }
  ```
- **Sample Response:**
  ```json
  { "message": "Notification sent successfully" }
  ```

#### **Chat**

##### **GET /api/chats/:room**

- **Description:**  
  Retrieve chat history for a specific room.
- **URL Parameter:**  
  - `room` – The chat room identifier.
- **Sample Request:**
  ```
  GET /api/chats/room123
  ```
- **Sample Response:**
  ```json
  [
    { "id": 1, "room": "room123", "sender": "user_1", "message": "Hello!", "createdAt": "2023-03-01T00:00:00.000Z" },
    ...
  ]
  ```

##### **POST /api/chats/send**

- **Description:**  
  Send a chat message and broadcast it to the room via Socket.IO.
- **Request Body (JSON):**
  ```json
  {
    "room": "room123",
    "sender": "user_1",
    "message": "Hello, how can I help you?"
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "room": "room123",
    "sender": "user_1",
    "message": "Hello, how can I help you?",
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

---

### 15. Advanced Reporting (Dashboard)

#### **GET /api/reporting/advanced**

- **Description:**  
  Retrieve advanced analytical reports including:
  - Conversion rate (bookings with transaction_type 'direct_purchase')
  - Average booking price
  - Total revenue (sum of prices for booked houses)
  - Booking distribution by transaction type
  - Heatmap data (number of houses per location)
- **Sample Request:**
  ```
  GET /api/reporting/advanced
  ```
- **Sample Response:**
  ```json
  {
    "conversionRate": "25.00%",
    "avgBookingPrice": "1200.00",
    "totalRevenue": "24000.00",
    "bookingDistribution": [
      { "transactionType": "direct_purchase", "count": 5 },
      { "transactionType": "rental", "count": 15 }
    ],
    "heatmapData": [
      { "area_name": "Downtown", "houseCount": 10 },
      { "area_name": "Uptown", "houseCount": 8 }
    ]
  }
  ```

---

### 16. User Profile Management

#### **PUT /api/users/profile/:id**

- **Description:**  
  Update a comprehensive user profile including additional fields such as address, phone, buyingHistory, and sellingHistory. Sensitive fields (email, phone) are validated.
- **URL Parameter:**  
  - `id` – User ID.
- **Request Body (JSON):**
  ```json
  {
    "email": "newemail@example.com",
    "phone": "09123456789",
    "address": "456 New Address",
    "buyingHistory": ["House 1", "House 2"],
    "sellingHistory": ["House 3"]
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "newemail@example.com",
    "phone": "09123456789",
    "address": "456 New Address",
    "buyingHistory": ["House 1", "House 2"],
    "sellingHistory": ["House 3"],
    "updatedAt": "2023-04-01T00:00:00.000Z"
  }
  ```

---

## Setup & Installation

1. **Install Dependencies:**  
   Make sure Node.js and npm/yarn are installed. Run:
   ```bash
   npm install
   ```
   or
   ```bash
   yarn install
   ```

2. **Configure Environment Variables:**  
   Create a `.env` file in the project root and define variables such as:
   - `PORT`
   - `ACCESS_TOKEN_SECRET`
   - `REFRESH_TOKEN_SECRET`
   - `FACEBOOK_APP_ID`, `FACEBOOK_APP_SECRET`, `FACEBOOK_CALLBACK_URL`
   - `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `GOOGLE_CALLBACK_URL`
   - `LINKEDIN_CLIENT_ID`, `LINKEDIN_CLIENT_SECRET`, `LINKEDIN_CALLBACK_URL`
   - Database connection settings

3. **Compile TypeScript (if not using ts-node):**  
   ```bash
   tsc
   ```

4. **Start the Server:**  
   ```bash
   npm start
   ```
   The server will run on the port specified in your environment variables (default is 3000).

---

## Usage Examples

- **Listing Houses:**  
  `GET /api/houses?page=1&limit=10&sort=price&order=DESC`  
  Retrieves a paginated list of houses sorted by price in descending order.

- **Advanced Geo-Search:**  
  `GET /api/houses/geo-search?lat=35.6892&lng=51.3890&radius=5000`  
  Retrieves houses within a 5km radius of the given coordinates.

- **User Login:**  
  `POST /api/users/login` with body:
  ```json
  { "email": "user@example.com", "password": "securePassword" }
  ```
  Returns a JWT token for authenticated requests.

- **Initiate Payment:**  
  `POST /api/payments/checkout` with body:
  ```json
  { "bookingId": 1, "amount": 1200 }
  ```
  Returns a payment URL and authority code.

- **Send Real-time Notification (via Socket.IO):**  
  Use the `sendNotification` event in Socket.IO (see socket implementation) to send notifications to users.

- **Social Authentication:**  
  Access endpoints like `GET /api/auth/social/facebook` to log in via social networks.

- **Update User Profile:**  
  `PUT /api/users/profile/1` with body:
  ```json
  {
    "email": "newemail@example.com",
    "phone": "09123456789",
    "address": "456 New Address",
    "buyingHistory": ["House 1", "House 2"],
    "sellingHistory": ["House 3"]
  }
  ```

---

## Future Enhancements

- **Real-time Analytics:** Integrate real-time data streams for analytics dashboards.
- **Advanced Search Filters:** Enhance search functionality with more detailed filters and full-text search.
- **Push Notifications:** Integrate with services like Firebase Cloud Messaging for mobile push notifications.
- **Chat Enhancements:** Add features such as file sharing, typing indicators, and read receipts in the chat system.
- **Blockchain Integration:** For secure, transparent property ownership tracking.
- **Machine Learning Recommendations:** Implement recommendation systems for properties based on user behavior.

---

This documentation provides a comprehensive guide to all API endpoints, their usage, expected inputs, and response examples. It covers the complete functionality of the Real Estate API, ensuring that both developers and API consumers have the necessary information to interact with the system effectively.
```

---

You can save the above content as your `README.md` file on GitHub. If you need further modifications or additional sections, let me know!
