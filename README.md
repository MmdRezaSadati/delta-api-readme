
# Real Estate API Documentation

## Introduction

This API provides a comprehensive set of endpoints for managing real estate properties, bookings, user interactions, payments, and more. It supports advanced features such as geo-search, real-time notifications, social authentication, intelligent recommendations, and gamification. The documentation below details each endpoint, its parameters, expected inputs/outputs, and usage scenarios.

> **Note:** This documentation is intended solely for frontend developers to understand how to interact with the API. It does not include installation or client integration instructions.

---

## Table of Contents

1. [Properties (Houses)](#1-properties-houses)
2. [Users](#2-users)
3. [Bookings](#3-bookings)
4. [Comments, Ratings & Feedback](#4-comments-ratings--feedback)
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
- **Description:** Retrieve all houses with filtering, pagination, and sorting.
- **Query Parameters:**  
  - `page` (optional, default: "1")
  - `limit` (optional, default: "10")
  - `sort` (optional) – Field to sort by (e.g., "price", "rate")
  - `order` (optional, default: "ASC")
  - Additional filters: `title`, `address`, `rate`, `price`, `capacity`, `transaction_type`
- **Usage Scenario:** Display a list of properties based on search criteria.
- **Sample Request:**  
  `GET /api/houses?page=1&limit=10&sort=price&order=DESC&transaction_type=rental`
- **Sample Response:**  
  ```json
  [
    {
      "id": 1,
      "title": "آپارتمان مدرن",
      "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
      "rate": 4.5,
      "price": 1200000,
      "capacity": 4,
      "transaction_type": "اجاره",
      "createdAt": "2023-01-01T00:00:00.000Z"
    },
    ...
  ]
  ```

### **GET /api/houses/:id**
- **Description:** Retrieve detailed information about a specific house.
- **URL Parameter:**  
  - `id`: House identifier.
- **Usage Scenario:** When a user selects a property, this endpoint provides all details.
- **Sample Request:**  
  `GET /api/houses/1`
- **Sample Response:**  
  ```json
  {
    "id": 1,
    "title": "آپارتمان مدرن",
    "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
    "rate": 4.5,
    "price": 1200000,
    "capacity": 4,
    "transaction_type": "اجاره",
    "photos": ["https://via.placeholder.com/640x480?text=آپارتمان+مدرن+1", "https://via.placeholder.com/640x480?text=آپارتمان+مدرن+2"],
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### **POST /api/houses**
- **Description:** Create a new house.
- **Request Body:** (JSON)
  ```json
  {
    "title": "آپارتمان مدرن",
    "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
    "rate": 4.5,
    "price": 1200000,
    "capacity": 4,
    "transaction_type": "اجاره",
    "photos": ["https://via.placeholder.com/640x480?text=آپارتمان+مدرن+1", "https://via.placeholder.com/640x480?text=آپارتمان+مدرن+2"],
    "tags": ["مدرن", "آپارتمان"],
    "location": { "lat": 35.6892, "lng": 51.3890 },
    "categories": { "categories": ["مسکونی"] }
  }
  ```
- **Usage Scenario:** Used by property managers to add new properties.
- **Sample Response:**  
  ```json
  {
    "id": 1,
    "title": "آپارتمان مدرن",
    "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
    "rate": 4.5,
    "price": 1200000,
    "capacity": 4,
    "transaction_type": "اجاره",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### **PUT /api/houses/:id**
- **Description:** Update an existing house.
- **URL Parameter:**  
  - `id`: House identifier.
- **Request Body:** (JSON)
  ```json
  {
    "price": 1300000,
    "capacity": 5
  }
  ```
- **Usage Scenario:** For correcting or updating property details.
- **Sample Response:**  
  ```json
  {
    "id": 1,
    "title": "آپارتمان مدرن",
    "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
    "rate": 4.5,
    "price": 1300000,
    "capacity": 5,
    "transaction_type": "اجاره",
    "updatedAt": "2023-02-01T00:00:00.000Z"
  }
  ```

### **DELETE /api/houses/:id**
- **Description:** Delete a house by its ID.
- **URL Parameter:**  
  - `id`: House identifier.
- **Usage Scenario:** Remove properties that are no longer available.
- **Sample Response:**  
  ```json
  { "message": "House deleted successfully" }
  ```

### **GET /api/houses/by-rating**
- **Description:** Retrieve houses sorted by their average rating (computed from user reviews).
- **Usage Scenario:** Used to display properties based on user feedback and ratings.
- **Sample Request:**  
  `GET /api/houses/by-rating`
- **Sample Response:**  
  ```json
  [
    { "id": 3, "title": "ویلا لوکس", "avgRating": 4.8, ... },
    { "id": 1, "title": "آپارتمان مدرن", "avgRating": 4.5, ... }
  ]
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
    "name": "علی کاربر"
  }
  ```
- **Usage Scenario:** User signup.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "name": "علی کاربر",
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
- **Usage Scenario:** User logs in and receives an access token and a refresh token.
- **Sample Response:**
  ```json
  { "accessToken": "jwt_access_token", "refreshToken": "jwt_refresh_token" }
  ```

### **POST /api/auth/refresh**
- **Description:** Refresh the access token using a valid refresh token.
- **Request Body:** (JSON)
  ```json
  { "token": "your_refresh_token_here" }
  ```
- **Usage Scenario:** When an access token expires, a new access token is obtained.
- **Sample Response:**
  ```json
  { "accessToken": "new_jwt_access_token" }
  ```

### **POST /api/auth/logout**
- **Description:** Log out a user by invalidating the refresh token.
- **Request Body:** (JSON)
  ```json
  { "token": "your_refresh_token_here" }
  ```
- **Usage Scenario:** Terminate user session.
- **Sample Response:**
  ```json
  { "message": "Logout successful" }
  ```

### **GET /api/users**
- **Description:** Retrieve all users with optional filtering by role or registration date.
- **Usage Scenario:** Admin view for managing users.
- **Sample Response:**
  ```json
  [
    { "id": 1, "email": "user@example.com", "role": "buyer", "name": "علی کاربر" },
    ...
  ]
  ```

### **GET /api/users/:id**
- **Description:** Retrieve details of a specific user.
- **Usage Scenario:** Display a user's detailed profile.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "name": "علی کاربر",
    "profile": {
      "address": "خیابان اصلی، تهران",
      "phone": "09123456789",
      "buyingHistory": [],
      "sellingHistory": []
    }
  }
  ```

### **PUT /api/users/:id**
- **Description:** Update a user's information (rehashes password if provided).
- **Usage Scenario:** User profile update.
- **Sample Response:**
  ```json
  { "id": 1, "email": "user@example.com", "name": "علی کاربر به‌روز شده" }
  ```

### **DELETE /api/users/:id**
- **Description:** Delete a user by their ID.
- **Usage Scenario:** Admin deletes a user.
- **Sample Response:**
  ```json
  { "message": "User deleted successfully" }
  ```

### **PUT /api/users/profile/:id**
- **Description:** Update a comprehensive public user profile.
- **Request Body:** (JSON)
  ```json
  {
    "email": "newemail@example.com",
    "phone": "09123456789",
    "address": "خیابان 456، تهران",
    "buyingHistory": ["خانه ۱", "خانه ۲"],
    "sellingHistory": ["خانه ۳"]
  }
  ```
- **Usage Scenario:** Users update their public profile details.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "newemail@example.com",
    "phone": "09123456789",
    "address": "خیابان 456، تهران",
    "buyingHistory": ["خانه ۱", "خانه ۲"],
    "sellingHistory": ["خانه ۳"],
    "updatedAt": "2023-04-01T00:00:00.000Z"
  }
  ```

---

## 3. Bookings

### **GET /api/bookings**
- **Description:** Retrieve all bookings with optional filtering (e.g., by house_id), pagination, and sorting.
- **Usage Scenario:** Display booking history.
- **Sample Response:**
  ```json
  [
    { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "سارا" } },
    ...
  ]
  ```

### **GET /api/bookings/:id**
- **Description:** Retrieve details of a specific booking.
- **Usage Scenario:** View detailed booking information.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "سارا" } }
  ```

### **POST /api/bookings**
- **Description:** Create a new booking.
- **Request Body:** (JSON)
  ```json
  {
    "house_id": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": { "name": "سارا", "contact": "09123456789" }
  }
  ```
- **Usage Scenario:** User reserves a property.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "سارا", "contact": "09123456789" } }
  ```

### **PUT /api/bookings/:id**
- **Description:** Update booking details.
- **Request Body:** (JSON)
  ```json
  { "reservedDates": ["2023-05-02", "2023-05-12"] }
  ```
- **Usage Scenario:** User updates their reservation dates.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-02", "2023-05-12"] }
  ```

### **DELETE /api/bookings/:id**
- **Description:** Delete a booking.
- **Usage Scenario:** Cancel a booking.
- **Sample Response:**
  ```json
  { "message": "Booking deleted successfully" }
  ```

---

## 4. Comments, Ratings & Feedback

### **POST /api/feedback**
- **Description:** Create a new feedback entry (review and loyalty points).
- **Request Body:** (JSON)
  ```json
  {
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "معامله عالی بود!",
    "pointsAwarded": 10
  }
  ```
- **Usage Scenario:** A seller leaves feedback for a buyer, awarding loyalty points.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "معامله عالی بود!",
    "pointsAwarded": 10,
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

### **GET /api/feedback/received/:userId**
- **Description:** Retrieve all feedback received by a user and total loyalty points earned.
- **Usage Scenario:** Display feedback and loyalty summary on a user's dashboard.
- **Sample Response:**
  ```json
  {
    "feedbacks": [
      { "id": 1, "rating": 4.5, "comment": "معامله عالی بود!", "pointsAwarded": 10 },
      ...
    ],
    "totalPoints": 10
  }
  ```

### **GET /api/feedback/given/:userId**
- **Description:** Retrieve all feedback given by a user.
- **Usage Scenario:** View the reviews a user has submitted.
- **Sample Response:**
  ```json
  [
    { "id": 1, "rating": 4.5, "comment": "معامله عالی بود!", "pointsAwarded": 10 },
    ...
  ]
  ```

### **GET /api/feedback/loyalty/:userId**
- **Description:** Retrieve the total loyalty points accumulated by a user.
- **Usage Scenario:** Show user's loyalty score.
- **Sample Response:**
  ```json
  { "userId": 3, "totalPoints": 25 }
  ```

---

## 5. Payment & Reservations

### **POST /api/payments/checkout**
- **Description:** Initiate payment for a booking.
- **Request Body:** (JSON)
  ```json
  { "bookingId": 1, "amount": 1200000 }
  ```
- **Usage Scenario:** User initiates a payment process.
- **Sample Response:**
  ```json
  {
    "authority": "FAKE_AUTH_CODE_12345",
    "paymentUrl": "https://payment-gateway.com/pay/FAKE_AUTH_CODE_12345"
  }
  ```

### **GET /api/payments/status/:transactionId**
- **Description:** Check the status of a payment.
- **URL Parameter:**  
  - `transactionId`: The transaction identifier.
- **Usage Scenario:** Verify if a payment has been completed.
- **Sample Response:**
  ```json
  { "status": "paid" }
  ```

### **POST /api/payments/verify**
- **Description:** Verify a payment after processing.
- **Request Body:** (JSON)
  ```json
  { "authority": "FAKE_AUTH_CODE_12345", "status": "OK", "bookingId": 1 }
  ```
- **Usage Scenario:** Confirm payment and update booking status.
- **Sample Response:**
  ```json
  { "message": "Payment successful and booking updated" }
  ```

---

## 6. Blogs

### **GET /api/blogs**
- **Description:** Retrieve all blog posts.
- **Usage Scenario:** Display blog content.
- **Sample Response:**
  ```json
  [
    { "id": 1, "title": "روندهای املاک", "author_id": 2, "category_id": 3, "createdAt": "2023-01-01T00:00:00.000Z" },
    ...
  ]
  ```

### **GET /api/blogs/:id**
- **Description:** Retrieve a specific blog post.
- **Usage Scenario:** View detailed blog post.
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3 }
  ```

### **POST /api/blogs**
- **Description:** Create a new blog post.
- **Request Body:** (JSON)
  ```json
  {
    "title": "روندهای جدید در املاک",
    "content": "متن مقاله...",
    "author_id": 2,
    "category_id": 3
  }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای جدید در املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3, "createdAt": "2023-03-01T00:00:00.000Z" }
  ```

### **PUT /api/blogs/:id**
- **Description:** Update an existing blog post.
- **Request Body:** (JSON)
  ```json
  { "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده..." }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده...", "updatedAt": "2023-03-05T00:00:00.000Z" }
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
    { "id": 1, "name": "مسکونی" },
    { "id": 2, "name": "تجاری" }
  ]
  ```

### **GET /api/categories/:id**
- **Description:** Retrieve details of a specific category.
- **Sample Response:**
  ```json
  { "id": 1, "name": "مسکونی" }
  ```

### **POST /api/categories**
- **Description:** Create a new category.
- **Request Body:** (JSON)
  ```json
  { "name": "لوکس" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "name": "لوکس" }
  ```

### **PUT /api/categories/:id**
- **Description:** Update an existing category.
- **Request Body:** (JSON)
  ```json
  { "name": "دسته‌بندی بروزرسانی شده" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "name": "دسته‌بندی بروزرسانی شده" }
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
- **Description:** Retrieve all favorite houses for a user.
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
- **Sample Response:**
  ```json
  [
    { "id": 1, "area_name": "تهران، منطقه مرکزی", "lat": 35.6892, "lng": 51.3890 },
    { "id": 2, "area_name": "اصفهان، منطقه تاریخی", "lat": 32.6546, "lng": 51.6680 },
    { "id": 3, "area_name": "شیراز، منطقه قدیمی", "lat": 29.5918, "lng": 52.5836 }
  ]
  ```

### **GET /api/locations/:id**
- **Description:** Retrieve details of a specific location.
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "تهران، منطقه مرکزی", "lat": 35.6892, "lng": 51.3890 }
  ```

### **POST /api/locations**
- **Description:** Create a new location.
- **Request Body:** (JSON)
  ```json
  { "area_name": "شهر جدید", "lat": 35.7000, "lng": 51.4000 }
  ```
- **Sample Response:**
  ```json
  { "id": 4, "area_name": "شهر جدید", "lat": 35.7000, "lng": 51.4000 }
  ```

### **PUT /api/locations/:id**
- **Description:** Update a location.
- **Request Body:** (JSON)
  ```json
  { "area_name": "منطقه بروزرسانی شده" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "منطقه بروزرسانی شده", "lat": 35.6892, "lng": 51.3890 }
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
- **Description:** Retrieve all social media links.
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

### Social Login Endpoints
- **GET /api/auth/social/facebook**  
  Redirects to Facebook for authentication.
- **GET /api/auth/social/facebook/callback**  
  Callback endpoint after Facebook authentication.
- **GET /api/auth/social/google**  
  Redirects to Google for authentication.
- **GET /api/auth/social/google/callback**  
  Callback endpoint after Google authentication.
- **GET /api/auth/social/linkedin**  
  Redirects to LinkedIn for authentication.
- **GET /api/auth/social/linkedin/callback**  
  Callback endpoint after LinkedIn authentication.

*Note:* These endpoints are managed by Passport.js and on successful authentication return a JWT token.

---

## 12. Social Sharing

### **GET /api/share/property**
- **Description:** Generate a shareable URL for a property.
- **Query Parameter:**  
  - `propertyId`: The property ID.
- **Sample Request:**  
  `GET /api/share/property?propertyId=1`
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/properties/1" }
  ```

### **GET /api/share/comment**
- **Description:** Generate a shareable URL for a comment.
- **Query Parameter:**  
  - `commentId`: The comment ID.
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
  - `lat` (required): Latitude.
  - `lng` (required): Longitude.
  - `radius` (required): Radius in meters.
- **Sample Request:**  
  `GET /api/houses/geo-search?lat=35.6892&lng=51.3890&radius=5000`
- **Sample Response:**
  ```json
  [
    { "id": 3, "title": "خانه در مرکز شهر", ... },
    ...
  ]
  ```

---

## 14. Real-time Notifications & Chat

### Notifications

#### **POST /api/notifications/send**
- **Description:** Send a real-time push notification to a specific room via Socket.IO.
- **Request Body:** (JSON)
  ```json
  {
    "room": "user_1",
    "notification": "رزرو شما تایید شد!"
  }
  ```
- **Sample Response:**
  ```json
  { "message": "Notification sent successfully" }
  ```

### Chat

#### **GET /api/chats/:room**
- **Description:** Retrieve chat history for a specified room.
- **URL Parameter:**  
  - `room`: Chat room identifier.
- **Sample Request:**  
  `GET /api/chats/room123`
- **Sample Response:**
  ```json
  [
    { "id": 1, "room": "room123", "sender": "user_1", "message": "سلام!", "createdAt": "2023-03-01T00:00:00.000Z" },
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
    "message": "سلام، چگونه می‌توانم کمکتان کنم؟"
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "room": "room123",
    "sender": "user_1",
    "message": "سلام، چگونه می‌توانم کمکتان کنم؟",
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

---

## 15. Advanced Reporting (Dashboard)

### **GET /api/reporting/advanced**
- **Description:** Retrieve advanced analytical reports including:
  - **Conversion Rate:** Percentage of bookings with transaction type "direct_purchase".
  - **Average Booking Price:** Average price of booked houses.
  - **Total Revenue:** Sum of prices for booked houses.
  - **Booking Distribution:** Count of bookings grouped by transaction type.
  - **Heatmap Data:** Number of houses per location (grouped by area_name).
- **Sample Request:**  
  `GET /api/reporting/advanced`
- **Sample Response:**
  ```json
  {
    "conversionRate": "25.00%",
    "avgBookingPrice": "1200000.00",
    "totalRevenue": "24000000.00",
    "bookingDistribution": [
      { "transactionType": "direct_purchase", "count": 5 },
      { "transactionType": "rental", "count": 15 }
    ],
    "heatmapData": [
      { "area_name": "تهران، منطقه مرکزی", "houseCount": 10 },
      { "area_name": "اصفهان، منطقه تاریخی", "houseCount": 8 }
    ]
  }
  ```

---

## 16. User Profile Management

### **PUT /api/users/profile/:id**
- **Description:** Update a comprehensive user profile with additional details.
- **URL Parameter:**  
  - `id`: User identifier.
- **Request Body:** (JSON)
  ```json
  {
    "email": "newemail@example.com",
    "phone": "09123456789",
    "address": "خیابان 456، تهران",
    "buyingHistory": ["خانه ۱", "خانه ۲"],
    "sellingHistory": ["خانه ۳"]
  }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "newemail@example.com",
    "phone": "09123456789",
    "address": "خیابان 456، تهران",
    "buyingHistory": ["خانه ۱", "خانه ۲"],
    "sellingHistory": ["خانه ۳"],
    "updatedAt": "2023-04-01T00:00:00.000Z"
  }
  ```

---

## 17. Recommendation System

### **GET /api/recommendations/:userId**
- **Description:** Retrieve property recommendations for a user based on historical data and behavior.
- **URL Parameter:**  
  - `userId`: The ID of the user.
- **Usage Scenario:** Provide intelligent property suggestions (dummy implementation returns top 5 cheapest houses).
- **Sample Response:**
  ```json
  {
    "userId": 5,
    "recommendations": [
      { "id": 3, "title": "خانه اقتصادی", "price": 900000, ... },
      ...
    ]
  }
  ```

### **POST /api/recommendations/predict**
- **Description:** Predict the price of a property based on its features.
- **Request Body:** (JSON)
  ```json
  { "size": 120, "rooms": 3 }
  ```
- **Usage Scenario:** Assist users with an estimated price prediction (dummy implementation using a simple formula).
- **Sample Response:**
  ```json
  { "predictedPrice": 190000 }
  ```

---

## 18. Gamification & Feedback

### **POST /api/feedback**
- **Description:** Create a new feedback entry that includes a review and loyalty points.
- **Request Body:** (JSON)
  ```json
  {
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "معامله عالی بود!",
    "pointsAwarded": 10
  }
  ```
- **Usage Scenario:** A user (e.g., seller) provides feedback for another user (e.g., buyer) and awards loyalty points.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "reviewerId": 2,
    "revieweeId": 3,
    "rating": 4.5,
    "comment": "معامله عالی بود!",
    "pointsAwarded": 10,
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

### **GET /api/feedback/received/:userId**
- **Description:** Retrieve all feedback received by a user along with total loyalty points earned.
- **URL Parameter:**  
  - `userId`: The ID of the user receiving feedback.
- **Sample Response:**
  ```json
  {
    "feedbacks": [
      { "id": 1, "rating": 4.5, "comment": "معامله عالی بود!", "pointsAwarded": 10 },
      ...
    ],
    "totalPoints": 10
  }
  ```

### **GET /api/feedback/given/:userId**
- **Description:** Retrieve all feedback submitted by a user.
- **URL Parameter:**  
  - `userId`: The ID of the user who provided feedback.
- **Sample Response:**
  ```json
  [
    { "id": 1, "rating": 4.5, "comment": "معامله عالی بود!", "pointsAwarded": 10 },
    ...
  ]
  ```

### **GET /api/feedback/loyalty/:userId**
- **Description:** Retrieve the total loyalty points accumulated by a user.
- **URL Parameter:**  
  - `userId`: The user’s unique identifier.
- **Sample Response:**
  ```json
  { "userId": 3, "totalPoints": 25 }
  ```

---

## Scenarios

1. **User Registration & Login:**  
   A new user signs up via the registration endpoint and logs in to receive JWT tokens for authentication.

2. **Property Browsing & Advanced Search:**  
   Users browse properties with various filters and sort options. They can use geo-search to find properties within a geographical radius or sort properties by average rating using `/api/houses/by-rating`.

3. **Booking & Payment Processing:**  
   When a user books a property, they initiate payment via `/api/payments/checkout`. After payment processing, the payment is verified with `/api/payments/verify` and booking status is updated accordingly.

4. **Feedback & Loyalty System:**  
   After transactions, users provide feedback (reviews and ratings) via `/api/feedback`. The system accumulates loyalty points which users can later view through `/api/feedback/received/:userId` or `/api/feedback/loyalty/:userId`.

5. **Property Recommendations & Price Prediction:**  
   Based on user behavior and historical data, the recommendation system suggests properties via `/api/recommendations/:userId` and provides price predictions using `/api/recommendations/predict`.

6. **Real-time Communication:**  
   The API supports real-time notifications (via `/api/notifications/send`) and chat functionality (via `/api/chats/*`) to enable instant communication between users, such as between a buyer and seller.

7. **Social Integration:**  
   Users can log in using their social accounts (Facebook, Google, LinkedIn) and share properties or comments via social sharing endpoints.

8. **Advanced Reporting:**  
   Administrators can retrieve comprehensive analytical reports on bookings, revenue, and property distribution using `/api/reporting/advanced`.

9. **User Profile Management:**  
   Users can update their public profiles via `/api/users/profile/:id` to reflect changes in contact information, address, and transaction history.

10. **Gamification:**  
    The combined feedback system allows users to earn loyalty points based on their activities (e.g., leaving feedback, making bookings) and view their accumulated points.

---

This documentation provides a complete and detailed overview of all available API endpoints and usage scenarios for the Real Estate API. Frontend developers can refer to this guide to understand the functionalities and data flows in the system.

This documentation covers all endpoints and scenarios in detail, providing a complete picture of how the API functions. Frontend developers can refer to this guide to understand the available resources and how to interact with the backend system effectively.

```

---

This README.md file is intended to be a comprehensive reference for all API endpoints and scenarios for your Real Estate API project. If you need further customization or additional sections, let me know!
