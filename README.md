Below is a comprehensive API documentation in Markdown format intended for frontend developers. This document details every API endpoint, its parameters, expected inputs/outputs, and usage scenarios without including installation or frontend integration instructions.

---

```markdown
# Real Estate API Documentation

## Introduction

This API provides a comprehensive set of endpoints for managing real estate properties, bookings, user interactions, payments, and more. The API supports advanced features such as geo-search, real-time notifications, social authentication, intelligent recommendations, and a gamification system. This documentation describes each endpoint, the required parameters, and scenarios so that frontend developers have a complete understanding of how to interact with the backend.

---

## Table of Contents

1. [Properties (Houses)](#1-properties-houses)
2. [Users](#2-users)
3. [Bookings](#3-bookings)
4. [Comments & Ratings / Feedback](#4-comments--ratings--feedback)
5. [Payment & Reservations](#5-payment--reservations)
6. [Blogs](#6-blogs)
7. [Categories](#7-categories)
8. [Favorites](#8-favorites)
9. [Locations](#9-locations)
10. [Social Media Links](#10-social-media-links)
11. [Social Authentication](#11-social-authentication)
12. [Social Sharing](#12-social-sharing)
13. [Advanced Geo-Search](#13-advanced-geo-search)
14. [Real-time Notifications & Chat](#14-real-time-notifications--chat)
15. [Advanced Reporting (Dashboard)](#15-advanced-reporting-dashboard)
16. [User Profile Management](#16-user-profile-management)
17. [Recommendation System](#17-recommendation-system)
18. [Gamification & Feedback](#18-gamification--feedback)

---

## 1. Properties (Houses)

### **GET /api/houses**

- **Description:** Retrieve all houses with support for filtering, pagination, and sorting.
- **Query Parameters:**
  - `page` (optional, default: "1"): Page number.
  - `limit` (optional, default: "10"): Number of records per page.
  - `sort` (optional): Field name to sort (e.g., "price", "rate").
  - `order` (optional, default: "ASC"): Sorting order ("ASC" or "DESC").
  - Additional filters: `title`, `address`, `rate`, `price`, `capacity`, `transaction_type`.
- **Scenario:** Used to display a list of properties to users based on specific criteria (e.g., show all rental properties sorted by price descending).
- **Sample Request:**  
  `GET /api/houses?page=1&limit=10&sort=price&order=DESC&transaction_type=rental`
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
      "createdAt": "2023-01-01T00:00:00.000Z"
    },
    ...
  ]
  ```

### **GET /api/houses/:id**

- **Description:** Retrieve detailed information about a specific house.
- **URL Parameter:**  
  - `id`: The house's unique identifier.
- **Scenario:** When a user selects a property, this endpoint provides all details for that property.
- **Sample Request:**  
  `GET /api/houses/1`
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
    "photos": ["photo1.jpg", "photo2.jpg"],
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### **POST /api/houses**

- **Description:** Create a new house.
- **Request Body:** (JSON)
  ```json
  {
    "title": "Modern Apartment",
    "address": "123 Main St",
    "rate": 4.5,
    "price": 1200,
    "capacity": 4,
    "transaction_type": "rental",
    "photos": ["photo1.jpg", "photo2.jpg"]
  }
  ```
- **Scenario:** Used by property managers or sellers to add a new property.
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
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### **PUT /api/houses/:id**

- **Description:** Update the details of an existing house.
- **URL Parameter:**  
  - `id`: The house's unique identifier.
- **Request Body:** (JSON)
  ```json
  {
    "price": 1300,
    "capacity": 5
  }
  ```
- **Scenario:** Used for correcting or updating property details.
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
    "updatedAt": "2023-02-01T00:00:00.000Z"
  }
  ```

### **DELETE /api/houses/:id**

- **Description:** Delete a house by its ID.
- **URL Parameter:**  
  - `id`: The house's unique identifier.
- **Scenario:** Allows property managers to remove properties that are no longer available.
- **Sample Response:**
  ```json
  { "message": "House deleted successfully" }
  ```

---

## 2. Users

### **POST /api/users/register**

- **Description:** Register a new user.
- **Request Body:** (JSON)
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword",
    "role": "buyer",
    "name": "John Doe"
  }
  ```
- **Scenario:** Used during user signup.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "name": "John Doe",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### **POST /api/users/login**

- **Description:** Authenticate a user and return JWT tokens.
- **Request Body:** (JSON)
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword"
  }
  ```
- **Scenario:** User logs in and receives an access token (for short-term access) and a refresh token (for obtaining new access tokens).
- **Sample Response:**
  ```json
  { "accessToken": "jwt_access_token", "refreshToken": "jwt_refresh_token" }
  ```

### **GET /api/users**

- **Description:** Retrieve all users, with optional filtering by role or registration date.
- **Query Parameters:**  
  - `role` (optional)
  - `membership_date` (optional)
  - `page`, `limit` for pagination.
- **Scenario:** Admin or user management view.
- **Sample Response:**
  ```json
  [
    { "id": 1, "email": "user@example.com", "role": "buyer", "name": "John Doe" },
    ...
  ]
  ```

### **GET /api/users/:id**

- **Description:** Retrieve details of a specific user.
- **URL Parameter:**  
  - `id`: The user’s unique identifier.
- **Scenario:** For displaying a user’s full profile (private or public as defined).
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
      "buyingHistory": [],
      "sellingHistory": []
    }
  }
  ```

### **PUT /api/users/:id**

- **Description:** Update a user's information.
- **Request Body:** (JSON)
  ```json
  {
    "name": "John Doe Updated",
    "password": "newSecurePassword"
  }
  ```
- **Scenario:** Allows users to update their personal details.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "name": "John Doe Updated"
  }
  ```

### **DELETE /api/users/:id**

- **Description:** Delete a user.
- **URL Parameter:**  
  - `id`: The user's unique identifier.
- **Scenario:** Used by admin for user removal.
- **Sample Response:**
  ```json
  { "message": "User deleted successfully" }
  ```

---

## 3. Bookings

### **GET /api/bookings**

- **Description:** Retrieve all bookings with filtering, pagination, and sorting.
- **Query Parameters:**  
  - `house_id` (optional) – filter by property.
  - `page`, `limit`, `sort`, `order` for pagination and sorting.
- **Scenario:** Display booking history for users or admin.
- **Sample Response:**
  ```json
  [
    { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "Alice" } },
    ...
  ]
  ```

### **GET /api/bookings/:id**

- **Description:** Retrieve details of a specific booking.
- **URL Parameter:**  
  - `id`: The booking's unique identifier.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "Alice" } }
  ```

### **POST /api/bookings**

- **Description:** Create a new booking.
- **Request Body:** (JSON)
  ```json
  {
    "house_id": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": { "name": "Alice", "contact": "09123456789" }
  }
  ```
- **Scenario:** When a user reserves a property.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "house_id": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": { "name": "Alice", "contact": "09123456789" }
  }
  ```

### **PUT /api/bookings/:id**

- **Description:** Update an existing booking.
- **Request Body:** (JSON)
  ```json
  {
    "reservedDates": ["2023-05-02", "2023-05-12"]
  }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-02", "2023-05-12"] }
  ```

### **DELETE /api/bookings/:id**

- **Description:** Delete a booking.
- **Sample Response:**
  ```json
  { "message": "Booking deleted successfully" }
  ```

---

## 4. Comments & Ratings / Feedback

### **POST /api/feedback**

- **Description:** Create a new feedback entry which includes both a comment and a loyalty (rating/points) value.
- **Request Body:** (JSON)
  ```json
  {
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "Great transaction!",
    "pointsAwarded": 10
  }
  ```
- **Scenario:** A seller leaves feedback for a buyer (or vice versa), awarding loyalty points.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "Great transaction!",
    "pointsAwarded": 10,
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

### **GET /api/feedback/received/:userId**

- **Description:** Retrieve all feedback received by a specific user along with the total loyalty points earned.
- **URL Parameter:**  
  - `userId`: The ID of the user receiving feedback.
- **Sample Response:**
  ```json
  {
    "feedbacks": [
      { "id": 1, "rating": 4.5, "comment": "Great transaction!", "pointsAwarded": 10 },
      ...
    ],
    "totalPoints": 10
  }
  ```

### **GET /api/feedback/given/:userId**

- **Description:** Retrieve all feedback given by a specific user.
- **URL Parameter:**  
  - `userId`: The ID of the user who provided feedback.
- **Sample Response:**
  ```json
  [
    { "id": 1, "rating": 4.5, "comment": "Great transaction!", "pointsAwarded": 10 },
    ...
  ]
  ```

### **GET /api/feedback/loyalty/:userId**

- **Description:** Retrieve aggregated loyalty points for a user.
- **URL Parameter:**  
  - `userId`: The user's unique identifier.
- **Sample Response:**
  ```json
  { "userId": 3, "totalPoints": 25 }
  ```

---

## 5. Payment & Reservations

### **POST /api/payments/checkout**

- **Description:** Initiate a payment process for a booking.
- **Request Body:** (JSON)
  ```json
  { "bookingId": 1, "amount": 1200 }
  ```
- **Scenario:** When a user initiates payment for a reservation.
- **Sample Response:**
  ```json
  {
    "authority": "FAKE_AUTH_CODE_12345",
    "paymentUrl": "https://payment-gateway.com/pay/FAKE_AUTH_CODE_12345"
  }
  ```

### **GET /api/payments/status/:transactionId**

- **Description:** Check the status of a payment transaction.
- **URL Parameter:**  
  - `transactionId`: The transaction identifier.
- **Sample Response:**
  ```json
  { "status": "paid" }
  ```

### **POST /api/payments/verify**

- **Description:** Verify a payment after the transaction is processed.
- **Request Body:** (JSON)
  ```json
  { "authority": "FAKE_AUTH_CODE_12345", "status": "OK", "bookingId": 1 }
  ```
- **Scenario:** Once the payment is completed at the gateway, this endpoint verifies it.
- **Sample Response:**
  ```json
  { "message": "Payment successful and booking updated" }
  ```

---

## 6. Blogs

### **GET /api/blogs**

- **Description:** Retrieve all blog posts with optional filtering.
- **Query Parameters:**  
  - `page`, `limit`, `sort`, `order` for pagination and sorting.
- **Scenario:** Display blog posts on the website.
- **Sample Response:**
  ```json
  [
    { "id": 1, "title": "Real Estate Trends", "author_id": 2, "category_id": 3, "createdAt": "2023-01-01T00:00:00.000Z" },
    ...
  ]
  ```

### **GET /api/blogs/:id**

- **Description:** Retrieve a specific blog post by its ID.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "Real Estate Trends",
    "content": "Detailed content here...",
    "author_id": 2,
    "category_id": 3,
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### **POST /api/blogs**

- **Description:** Create a new blog post.
- **Request Body:** (JSON)
  ```json
  {
    "title": "New Trends in Real Estate",
    "content": "Blog content here...",
    "author_id": 2,
    "category_id": 3
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "New Trends in Real Estate",
    "content": "Blog content here...",
    "author_id": 2,
    "category_id": 3,
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

### **PUT /api/blogs/:id**

- **Description:** Update an existing blog post.
- **Request Body:** (JSON)
  ```json
  {
    "title": "Updated Real Estate Trends",
    "content": "Updated content..."
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "Updated Real Estate Trends",
    "content": "Updated content...",
    "updatedAt": "2023-03-05T00:00:00.000Z"
  }
  ```

### **DELETE /api/blogs/:id**

- **Description:** Delete a blog post.
- **Sample Response:**
  ```json
  { "message": "Blog post deleted successfully" }
  ```

---

## 7. Categories

### **GET /api/categories**

- **Description:** Retrieve all categories.
- **Sample Response:**
  ```json
  [
    { "id": 1, "name": "Residential" },
    { "id": 2, "name": "Commercial" }
  ]
  ```

### **GET /api/categories/:id**

- **Description:** Retrieve a specific category.
- **Sample Response:**
  ```json
  { "id": 1, "name": "Residential" }
  ```

### **POST /api/categories**

- **Description:** Create a new category.
- **Request Body:** (JSON)
  ```json
  { "name": "Luxury" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "name": "Luxury" }
  ```

### **PUT /api/categories/:id**

- **Description:** Update an existing category.
- **Request Body:** (JSON)
  ```json
  { "name": "Updated Category Name" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "name": "Updated Category Name" }
  ```

### **DELETE /api/categories/:id**

- **Description:** Delete a category.
- **Sample Response:**
  ```json
  { "message": "Category deleted successfully" }
  ```

---

## 8. Favorites

### **POST /api/favorites/add**

- **Description:** Add a house to a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "user_id": 1, "house_id": 2 }
  ```

### **DELETE /api/favorites/remove**

- **Description:** Remove a house from a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Sample Response:**
  ```json
  { "message": "Favorite removed successfully" }
  ```

### **GET /api/favorites/:user_id**

- **Description:** Retrieve all favorite houses for a user, with pagination.
- **URL Parameter:**  
  - `user_id`: The user's unique identifier.
- **Sample Request:**
  `GET /api/favorites/1?page=1&limit=10`
- **Sample Response:**
  ```json
  [
    { "id": 1, "user_id": 1, "house_id": 2 },
    ...
  ]
  ```

---

## 9. Locations

### **GET /api/locations**

- **Description:** Retrieve all locations with optional filtering.
- **Query Parameters:**  
  - `page`, `limit`, etc.
- **Sample Response:**
  ```json
  [
    { "id": 1, "area_name": "Downtown", "lat": 35.6892, "lng": 51.3890 },
    { "id": 2, "area_name": "Uptown", "lat": 35.7000, "lng": 51.4000 }
  ]
  ```

### **GET /api/locations/:id**

- **Description:** Retrieve details of a specific location.
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "Downtown", "lat": 35.6892, "lng": 51.3890 }
  ```

### **POST /api/locations**

- **Description:** Create a new location.
- **Request Body:** (JSON)
  ```json
  { "area_name": "Midtown", "lat": 35.6850, "lng": 51.3750 }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "area_name": "Midtown", "lat": 35.6850, "lng": 51.3750 }
  ```

### **PUT /api/locations/:id**

- **Description:** Update an existing location.
- **Request Body:** (JSON)
  ```json
  { "area_name": "Updated Area" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "Updated Area", "lat": 35.6892, "lng": 51.3890 }
  ```

### **DELETE /api/locations/:id**

- **Description:** Delete a location.
- **Sample Response:**
  ```json
  { "message": "Location deleted successfully" }
  ```

---

## 10. Social Media Links

### **GET /api/social-media-links**

- **Description:** Retrieve all social media links with optional filtering.
- **Sample Response:**
  ```json
  [
    { "id": 1, "platform": "Facebook", "url": "https://facebook.com/realestate" },
    { "id": 2, "platform": "Twitter", "url": "https://twitter.com/realestate" }
  ]
  ```

### **GET /api/social-media-links/:id**

- **Description:** Retrieve details of a specific social media link.
- **Sample Response:**
  ```json
  { "id": 1, "platform": "Facebook", "url": "https://facebook.com/realestate" }
  ```

### **POST /api/social-media-links**

- **Description:** Create a new social media link.
- **Request Body:** (JSON)
  ```json
  { "platform": "Instagram", "url": "https://instagram.com/realestate" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "platform": "Instagram", "url": "https://instagram.com/realestate" }
  ```

### **PUT /api/social-media-links/:id**

- **Description:** Update an existing social media link.
- **Request Body:** (JSON)
  ```json
  { "url": "https://instagram.com/newrealestate" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "platform": "Instagram", "url": "https://instagram.com/newrealestate" }
  ```

### **DELETE /api/social-media-links/:id**

- **Description:** Delete a social media link.
- **Sample Response:**
  ```json
  { "message": "Social media link deleted successfully" }
  ```

---

## 11. Social Authentication

### **Social Login Endpoints**

- **GET /api/auth/social/facebook**  
  Redirects the user to Facebook for authentication.

- **GET /api/auth/social/facebook/callback**  
  Callback endpoint after Facebook authentication.

- **GET /api/auth/social/google**  
  Redirects the user to Google for authentication.

- **GET /api/auth/social/google/callback**  
  Callback endpoint after Google authentication.

- **GET /api/auth/social/linkedin**  
  Redirects the user to LinkedIn for authentication.

- **GET /api/auth/social/linkedin/callback**  
  Callback endpoint after LinkedIn authentication.

*Note:* These endpoints are handled by Passport.js and return a JWT token upon successful authentication.

---

## 12. Social Sharing

### **GET /api/share/property**

- **Description:** Generate a shareable URL for a property.
- **Query Parameter:**  
  - `propertyId`: ID of the property.
- **Sample Request:**  
  `GET /api/share/property?propertyId=1`
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/properties/1" }
  ```

### **GET /api/share/comment**

- **Description:** Generate a shareable URL for a comment.
- **Query Parameter:**  
  - `commentId`: ID of the comment.
- **Sample Request:**  
  `GET /api/share/comment?commentId=1`
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/comments/1" }
  ```

---

## 13. Advanced Geo-Search

### **GET /api/houses/geo-search**

- **Description:** Retrieve houses within a specified geographical radius using PostGIS functions.
- **Query Parameters:**  
  - `lat`: Latitude (required)
  - `lng`: Longitude (required)
  - `radius`: Search radius in meters (required)
- **Sample Request:**  
  `GET /api/houses/geo-search?lat=35.6892&lng=51.3890&radius=5000`
- **Sample Response:**
  ```json
  [
    { "id": 3, "title": "House in Downtown", ... },
    ...
  ]
  ```

---

## 14. Real-time Notifications & Chat

### **Notifications**

#### **POST /api/notifications/send**

- **Description:** Send a real-time push notification to a specific room (e.g., user ID) via Socket.IO.
- **Request Body:** (JSON)
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

### **Chat**

#### **GET /api/chats/:room**

- **Description:** Retrieve chat history for a specific room.
- **URL Parameter:**  
  - `room`: The identifier for the chat room.
- **Sample Request:**  
  `GET /api/chats/room123`
- **Sample Response:**
  ```json
  [
    { "id": 1, "room": "room123", "sender": "user_1", "message": "Hello!", "createdAt": "2023-03-01T00:00:00.000Z" },
    ...
  ]
  ```

#### **POST /api/chats/send**

- **Description:** Send a chat message to a room. The message is broadcast via Socket.IO.
- **Request Body:** (JSON)
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

## 15. Advanced Reporting (Dashboard)

### **GET /api/reporting/advanced**

- **Description:** Retrieve advanced analytical reports including:
  - **Conversion Rate:** Percentage of bookings with transaction type "direct_purchase".
  - **Average Booking Price:** Average price of houses that have been booked.
  - **Total Revenue:** Sum of prices for booked houses.
  - **Booking Distribution:** Count of bookings grouped by the transaction type.
  - **Heatmap Data:** Number of houses per location (grouped by area_name).
- **Sample Request:**  
  `GET /api/reporting/advanced`
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

## 16. User Profile Management

### **PUT /api/users/profile/:id**

- **Description:** Update a comprehensive user profile with additional details.
- **URL Parameter:**  
  - `id`: The user's unique identifier.
- **Request Body:** (JSON)
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

## 17. Recommendation System

### **GET /api/recommendations/:userId**

- **Description:** Retrieve property recommendations for a user based on historical data, preferences, and buying behavior.  
- **URL Parameter:**  
  - `userId`: The ID of the user for whom recommendations are generated.
- **Scenario:** Provides intelligent suggestions for properties (this is a dummy implementation returning the top 5 cheapest houses).
- **Sample Response:**
  ```json
  {
    "userId": 5,
    "recommendations": [
      { "id": 3, "title": "Affordable Apartment", "price": 900, ... },
      ...
    ]
  }
  ```

### **POST /api/recommendations/predict**

- **Description:** Predict the price of a property based on its features using a (dummy) neural network model.
- **Request Body:** (JSON)
  ```json
  { "size": 120, "rooms": 3 }
  ```
- **Scenario:** Helps users understand the expected price of a property based on given features.
- **Sample Response:**
  ```json
  { "predictedPrice": 190000 }
  ```

---

## 18. Gamification & Feedback

### **POST /api/feedback**

- **Description:** Create a new feedback entry which combines review and loyalty points.
- **Request Body:** (JSON)
  ```json
  {
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "Great transaction!",
    "pointsAwarded": 10
  }
  ```
- **Scenario:** A seller leaves feedback for a buyer, awarding loyalty points along with a rating and comment.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "Great transaction!",
    "pointsAwarded": 10,
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

### **GET /api/feedback/received/:userId**

- **Description:** Retrieve all feedback received by a specific user along with the total loyalty points earned.
- **URL Parameter:**  
  - `userId`: The ID of the user receiving feedback.
- **Sample Response:**
  ```json
  {
    "feedbacks": [
      { "id": 1, "rating": 4.5, "comment": "Great transaction!", "pointsAwarded": 10 },
      ...
    ],
    "totalPoints": 10
  }
  ```

### **GET /api/feedback/given/:userId**

- **Description:** Retrieve all feedback submitted by a specific user.
- **URL Parameter:**  
  - `userId`: The ID of the user who provided feedback.
- **Sample Response:**
  ```json
  [
    { "id": 1, "rating": 4.5, "comment": "Great transaction!", "pointsAwarded": 10 },
    ...
  ]
  ```

### **GET /api/feedback/loyalty/:userId**

- **Description:** Retrieve the total loyalty points accumulated by a user.
- **URL Parameter:**  
  - `userId`: The user's unique identifier.
- **Sample Response:**
  ```json
  { "userId": 3, "totalPoints": 25 }
  ```

---

## Scenarios

1. **User Registration & Login:**  
   A new user signs up using the registration endpoint. After registration, the user logs in and receives an access token and a refresh token, which will be used for authenticated API requests.

2. **Property Browsing:**  
   Users can browse properties with various filters (e.g., by price, address, transaction type) using the `/api/houses` endpoint. They can also perform advanced geo-search to locate properties within a specific radius.

3. **Booking & Payment:**  
   When a user makes a booking, they initiate a payment via `/api/payments/checkout`. After payment, the transaction is verified using `/api/payments/verify`, and the booking is updated accordingly.

4. **Feedback & Loyalty:**  
   After a transaction, a seller or buyer can leave feedback using the unified feedback endpoint. The system accumulates loyalty points based on the feedback, and users can view their total points and rewards through the loyalty endpoints.

5. **Property Recommendations:**  
   Based on user activity, the recommendation system provides suggestions for properties. The recommendation endpoint uses historical data and a dummy neural network model for price prediction.

6. **Real-time Interaction:**  
   The API supports real-time notifications and chat. When a significant event occurs (e.g., booking confirmation or payment update), a notification is sent via Socket.IO. Users can also communicate through the chat endpoints.

7. **Social Features:**  
   Users can log in via social authentication (Facebook, Google, LinkedIn) and share properties or comments on social media using the sharing endpoints.

8. **Advanced Reporting:**  
   Admins can access detailed analytical reports on bookings, revenue, and property distribution using the advanced reporting endpoints.

9. **User Profile Management:**  
   Users have a public profile that displays non-sensitive information. They can update their profile using the dedicated endpoint to reflect changes in address, contact info, and history.

10. **Gamification:**  
    Users earn loyalty points based on their activities (e.g., leaving feedback, booking properties) which can later be redeemed for rewards, such as discounts on future bookings.

---

This documentation covers all endpoints and scenarios in detail, providing a complete picture of how the API functions. Frontend developers can refer to this guide to understand the available resources and how to interact with the backend system effectively.

```

---

This README.md file is intended to be a comprehensive reference for all API endpoints and scenarios for your Real Estate API project. If you need further customization or additional sections, let me know!
