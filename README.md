# Real Estate API Documentation

## Introduction

This API provides a comprehensive set of endpoints for managing real estate properties and related user interactions. It covers property management, user accounts, bookings, payments, blogs, reviews & feedback, and additional features such as Q&A, price history tracking, property comparisons, saved searches, wishlists, availability calendars, seller upgrade processes, social sharing/bookmarking, and targeted notifications. This document details each API endpoint along with its parameters, expected inputs/outputs, and usage scenarios to guide frontend developers in integrating the API.

> **Note:** This documentation focuses solely on describing the API endpoints and their functionalities. It does not include instructions on how to integrate these APIs into the frontend (e.g., using Axios).

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
13. [Real-time Notifications & Chat](#13-real-time-notifications--chat)
14. [Advanced Reporting (Dashboard)](#14-advanced-reporting-dashboard)
15. [User Profile Management](#15-user-profile-management)
16. [Recommendation System](#16-recommendation-system)
17. [Q&A for Properties](#17-qa-for-properties)
18. [Price History Tracking](#18-price-history-tracking)
19. [Property Comparison](#19-property-comparison)
20. [Saved Searches](#20-saved-searches)
21. [Wishlists](#21-wishlists)
22. [Property Availability Calendar](#22-property-availability-calendar)
23. [Seller Upgrade](#23-seller-upgrade)
24. [Social Bookmark with Notes](#24-social-bookmark-with-notes)
25. [Targeted Notifications](#25-targeted-notifications)

---

## 1. Properties (Houses)

### GET /api/houses
- **Description:** Retrieves all houses with support for filtering, pagination, and sorting.
- **Query Parameters:**
  - `page` (optional, default: "1")
  - `limit` (optional, default: "10")
  - `sort` (optional): Field name to sort by (e.g., "price", "rate")
  - `order` (optional, default: "ASC"): Sorting order ("ASC" or "DESC")
  - Additional filters: `title`, `address`, `rate`, `price`, `capacity`, `transaction_type`
- **Usage Scenario:** Display a list of properties based on various criteria.
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

### GET /api/houses/:id
- **Description:** Retrieves detailed information about a specific house.
- **URL Parameter:** `id` (House identifier)
- **Usage Scenario:** When a user selects a property, this endpoint provides all its details.
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
    "photos": [
      "https://via.placeholder.com/640x480?text=آپارتمان+مدرن+1",
      "https://via.placeholder.com/640x480?text=آپارتمان+مدرن+2"
    ],
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### POST /api/houses
- **Description:** Creates a new house. The seller's information (sellerId, sellerName, sellerPicture) is automatically taken from the authenticated user's token.
- **Request Body:** (JSON)
  ```json
  {
    "title": "آپارتمان مدرن",
    "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
    "photos": [
      "https://via.placeholder.com/640x480?text=آپارتمان+مدرن+1",
      "https://via.placeholder.com/640x480?text=آپارتمان+مدرن+2"
    ],
    "rate": 4.5,
    "price": 1200000,
    "capacity": 4,
    "transaction_type": "اجاره",
    "tags": ["مدرن", "آپارتمان"],
    "location": { "lat": 35.6892, "lng": 51.3890 },
    "categories": { "categories": ["مسکونی"] }
  }
  ```
- **Usage Scenario:** Sellers add new properties; seller details are added from the token.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "آپارتمان مدرن",
    "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
    "sellerId": 10,
    "sellerName": "علی فروشنده",
    "sellerPicture": "https://via.placeholder.com/150?text=فروشنده",
    "rate": 4.5,
    "price": 1200000,
    "capacity": 4,
    "transaction_type": "اجاره",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### PUT /api/houses/:id
- **Description:** Updates an existing house.
- **URL Parameter:** `id` (House identifier)
- **Request Body:** (JSON)
  ```json
  {
    "price": 1300000,
    "capacity": 5
  }
  ```
- **Usage Scenario:** Sellers update property details.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "title": "آپارتمان مدرن",
    "address": "خیابان ولیعصر، تهران، منطقه مرکزی",
    "price": 1300000,
    "capacity": 5,
    "transaction_type": "اجاره",
    "updatedAt": "2023-02-01T00:00:00.000Z"
  }
  ```

### DELETE /api/houses/:id
- **Description:** Deletes a house.
- **URL Parameter:** `id` (House identifier)
- **Usage Scenario:** Remove properties that are no longer available.
- **Sample Response:**
  ```json
  { "message": "House deleted successfully" }
  ```

### GET /api/houses/by-rating
- **Description:** Retrieves houses sorted by their average rating (computed from user reviews).
- **Usage Scenario:** Display properties based on user feedback.
- **Sample Request:**  
  `GET /api/houses/by-rating`
- **Sample Response:**
  ```json
  [
    { "id": 3, "title": "ویلا لوکس", "avgRating": 4.8 },
    { "id": 1, "title": "آپارتمان مدرن", "avgRating": 4.5 }
  ]
  ```

### GET /api/houses/geo-search
- **Description:** Retrieves houses within a specified geographical radius using PostGIS functions.
- **Query Parameters:**
  - `lat` (required): Latitude.
  - `lng` (required): Longitude.
  - `radius` (required): Radius in meters.
- **Usage Scenario:** Users find properties within a specific area.
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

## 2. Users

### POST /api/users/register
- **Description:** Registers a new user with the default role "buyer". Any attempt to set a different role will be rejected.
- **Request Body:** (JSON)
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword",
    "name": "علی کاربر",
    "profilePicture": "https://via.placeholder.com/150?text=کاربر"
  }
  ```
- **Usage Scenario:** New user sign-up.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "name": "علی کاربر",
    "profilePicture": "https://via.placeholder.com/150?text=کاربر",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### POST /api/users/login
- **Description:** Authenticates a user and returns JWT tokens.
- **Request Body:** (JSON)
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword"
  }
  ```
- **Usage Scenario:** User login.
- **Sample Response:**
  ```json
  { "accessToken": "jwt_access_token", "refreshToken": "jwt_refresh_token" }
  ```

### POST /api/auth/refresh
- **Description:** Refreshes the access token using a valid refresh token.
- **Request Body:** (JSON)
  ```json
  { "token": "your_refresh_token_here" }
  ```
- **Usage Scenario:** Obtain a new access token when the old one expires.
- **Sample Response:**
  ```json
  { "accessToken": "new_jwt_access_token" }
  ```

### POST /api/auth/logout
- **Description:** Logs out the user by invalidating the refresh token.
- **Request Body:** (JSON)
  ```json
  { "token": "your_refresh_token_here" }
  ```
- **Usage Scenario:** End user session.
- **Sample Response:**
  ```json
  { "message": "Logout successful" }
  ```

### GET /api/users
- **Description:** Retrieves all users with optional filtering.
- **Usage Scenario:** User management view for admins.
- **Sample Response:**
  ```json
  [
    { "id": 1, "email": "user@example.com", "role": "buyer", "name": "علی کاربر" },
    ...
  ]
  ```

### GET /api/users/:id
- **Description:** Retrieves detailed information of a specific user.
- **Usage Scenario:** Display a user’s full profile.
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

### PUT /api/users/:id
- **Description:** Updates a user's information. If a password is provided, it will be re-hashed.
- **Request Body:** (JSON)
  ```json
  { "name": "علی کاربر به‌روز شده", "password": "newSecurePassword" }
  ```
- **Usage Scenario:** User profile update.
- **Sample Response:**
  ```json
  { "id": 1, "email": "user@example.com", "name": "علی کاربر به‌روز شده" }
  ```

### PUT /api/users/profile/:id
- **Description:** Updates the public profile of a user with additional details.
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
- **Usage Scenario:** User updates their public profile details.
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

### DELETE /api/users/:id
- **Description:** Deletes a user.
- **Usage Scenario:** Admin deletes a user.
- **Sample Response:**
  ```json
  { "message": "User deleted successfully" }
  ```

---

## 3. Bookings

### GET /api/bookings
- **Description:** Retrieves all bookings with filtering, pagination, and sorting.
- **Usage Scenario:** Display a list of bookings.
- **Sample Response:**
  ```json
  [
    { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "سارا" } },
    ...
  ]
  ```

### GET /api/bookings/:id
- **Description:** Retrieves details of a specific booking.
- **Usage Scenario:** View detailed booking information.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "سارا" } }
  ```

### POST /api/bookings
- **Description:** Creates a new booking.
- **Request Body:** (JSON)
  ```json
  {
    "house_id": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": { "name": "سارا", "contact": "09123456789", "userId": 1 }
  }
  ```
- **Usage Scenario:** When a user reserves a property.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "name": "سارا", "contact": "09123456789" } }
  ```

### PUT /api/bookings/:id
- **Description:** Updates an existing booking.
- **Request Body:** (JSON)
  ```json
  { "reservedDates": ["2023-05-02", "2023-05-12"] }
  ```
- **Usage Scenario:** User updates their reservation dates.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-02", "2023-05-12"] }
  ```

### DELETE /api/bookings/:id
- **Description:** Deletes a booking.
- **Usage Scenario:** Cancel a booking.
- **Sample Response:**
  ```json
  { "message": "Booking deleted successfully" }
  ```

---

## 4. Comments, Ratings & Feedback

### POST /api/feedback
- **Description:** Creates a new feedback entry that includes a review and loyalty points.
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
- **Usage Scenario:** A seller (or buyer) leaves feedback for the other party.
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

### GET /api/feedback/received/:userId
- **Description:** Retrieves all feedback received by a user along with total loyalty points earned.
- **Usage Scenario:** Show feedback and loyalty summary for a user.
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

### GET /api/feedback/given/:userId
- **Description:** Retrieves all feedback given by a user.
- **Usage Scenario:** View the feedback a user has submitted.
- **Sample Response:**
  ```json
  [
    { "id": 1, "rating": 4.5, "comment": "معامله عالی بود!", "pointsAwarded": 10 },
    ...
  ]
  ```

### GET /api/feedback/loyalty/:userId
- **Description:** Retrieves the total loyalty points accumulated by a user.
- **Usage Scenario:** Display a user’s total loyalty score.
- **Sample Response:**
  ```json
  { "userId": 3, "totalPoints": 25 }
  ```

---

## 5. Payment & Reservations

### POST /api/payments/checkout
- **Description:** Initiates a payment process for a booking.
- **Request Body:** (JSON)
  ```json
  { "bookingId": 1, "amount": 1200000 }
  ```
- **Usage Scenario:** User initiates a payment for a reservation.
- **Sample Response:**
  ```json
  {
    "authority": "FAKE_AUTH_CODE_12345",
    "paymentUrl": "https://payment-gateway.example.com/pay/FAKE_AUTH_CODE_12345"
  }
  ```

### GET /api/payments/status/:transactionId
- **Description:** Checks the status of a payment transaction.
- **URL Parameter:** `transactionId`
- **Usage Scenario:** Verify payment completion.
- **Sample Response:**
  ```json
  { "status": "paid" }
  ```

### POST /api/payments/verify
- **Description:** Verifies a payment after the transaction.
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

### GET /api/blogs
- **Description:** Retrieves all blog posts.
- **Usage Scenario:** Display blog content on the site.
- **Sample Response:**
  ```json
  [
    { "id": 1, "title": "روندهای املاک", "author_id": 2, "category_id": 3, "createdAt": "2023-01-01T00:00:00.000Z" },
    ...
  ]
  ```

### GET /api/blogs/:id
- **Description:** Retrieves a specific blog post.
- **Usage Scenario:** View detailed blog content.
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3 }
  ```

### POST /api/blogs
- **Description:** Creates a new blog post.
- **Request Body:** (JSON)
  ```json
  {
    "title": "روندهای جدید در املاک",
    "content": "متن مقاله...",
    "author_id": 2,
    "category_id": 3
  }
  ```
- **Usage Scenario:** Content creators add new blog posts.
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای جدید در املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3, "createdAt": "2023-03-01T00:00:00.000Z" }
  ```

### PUT /api/blogs/:id
- **Description:** Updates an existing blog post.
- **Request Body:** (JSON)
  ```json
  { "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده..." }
  ```
- **Usage Scenario:** Edit blog posts.
- **Sample Response:**
  ```json
  { "id": 1, "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده...", "updatedAt": "2023-03-05T00:00:00.000Z" }
  ```

### DELETE /api/blogs/:id
- **Description:** Deletes a blog post.
- **Usage Scenario:** Remove outdated blog content.
- **Sample Response:**
  ```json
  { "message": "Blog post deleted successfully" }
  ```

---

## 7. Categories

### GET /api/categories
- **Description:** Retrieves all categories.
- **Sample Response:**
  ```json
  [
    { "id": 1, "name": "مسکونی" },
    { "id": 2, "name": "تجاری" }
  ]
  ```

### GET /api/categories/:id
- **Description:** Retrieves details of a specific category.
- **Sample Response:**
  ```json
  { "id": 1, "name": "مسکونی" }
  ```

### POST /api/categories
- **Description:** Creates a new category.
- **Request Body:** (JSON)
  ```json
  { "name": "لوکس" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "name": "لوکس" }
  ```

### PUT /api/categories/:id
- **Description:** Updates an existing category.
- **Request Body:** (JSON)
  ```json
  { "name": "دسته‌بندی بروزرسانی شده" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "name": "دسته‌بندی بروزرسانی شده" }
  ```

### DELETE /api/categories/:id
- **Description:** Deletes a category.
- **Sample Response:**
  ```json
  { "message": "Category deleted successfully" }
  ```

---

## 8. Favorites

### POST /api/favorites/add
- **Description:** Adds a house to a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Usage Scenario:** User bookmarks a property.
- **Sample Response:**
  ```json
  { "id": 1, "user_id": 1, "house_id": 2 }
  ```

### DELETE /api/favorites/remove
- **Description:** Removes a house from a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Sample Response:**
  ```json
  { "message": "Favorite removed successfully" }
  ```

### GET /api/favorites/:user_id
- **Description:** Retrieves all favorite houses for a specific user.
- **URL Parameter:** `user_id`
- **Usage Scenario:** Display user’s saved properties.
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

### GET /api/locations
- **Description:** Retrieves all locations with optional filtering.
- **Sample Response:**
  ```json
  [
    { "id": 1, "area_name": "تهران، منطقه مرکزی", "lat": 35.6892, "lng": 51.3890 },
    { "id": 2, "area_name": "اصفهان، منطقه تاریخی", "lat": 32.6546, "lng": 51.6680 },
    { "id": 3, "area_name": "شیراز، منطقه قدیمی", "lat": 29.5918, "lng": 52.5836 }
  ]
  ```

### GET /api/locations/:id
- **Description:** Retrieves details of a specific location.
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "تهران، منطقه مرکزی", "lat": 35.6892, "lng": 51.3890 }
  ```

### POST /api/locations
- **Description:** Creates a new location.
- **Request Body:** (JSON)
  ```json
  { "area_name": "شهر جدید", "lat": 35.7000, "lng": 51.4000 }
  ```
- **Sample Response:**
  ```json
  { "id": 4, "area_name": "شهر جدید", "lat": 35.7000, "lng": 51.4000 }
  ```

### PUT /api/locations/:id
- **Description:** Updates an existing location.
- **Request Body:** (JSON)
  ```json
  { "area_name": "منطقه بروزرسانی شده" }
  ```
- **Sample Response:**
  ```json
  { "id": 1, "area_name": "منطقه بروزرسانی شده", "lat": 35.6892, "lng": 51.3890 }
  ```

### DELETE /api/locations/:id
- **Description:** Deletes a location.
- **Sample Response:**
  ```json
  { "message": "Location deleted successfully" }
  ```

---

## 10. Social Media Links

### GET /api/social-media-links
- **Description:** Retrieves all social media links.
- **Sample Response:**
  ```json
  [
    { "id": 1, "platform": "Facebook", "url": "https://facebook.com/realestate" },
    { "id": 2, "platform": "Twitter", "url": "https://twitter.com/realestate" }
  ]
  ```

### GET /api/social-media-links/:id
- **Description:** Retrieves details of a specific social media link.
- **Sample Response:**
  ```json
  { "id": 1, "platform": "Facebook", "url": "https://facebook.com/realestate" }
  ```

### POST /api/social-media-links
- **Description:** Creates a new social media link.
- **Request Body:** (JSON)
  ```json
  { "platform": "Instagram", "url": "https://instagram.com/realestate" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "platform": "Instagram", "url": "https://instagram.com/realestate" }
  ```

### PUT /api/social-media-links/:id
- **Description:** Updates an existing social media link.
- **Request Body:** (JSON)
  ```json
  { "url": "https://instagram.com/newrealestate" }
  ```
- **Sample Response:**
  ```json
  { "id": 3, "platform": "Instagram", "url": "https://instagram.com/newrealestate" }
  ```

### DELETE /api/social-media-links/:id
- **Description:** Deletes a social media link.
- **Sample Response:**
  ```json
  { "message": "Social media link deleted successfully" }
  ```

---

## 11. Social Authentication

### Social Login Endpoints
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

*Note:* These endpoints are managed by Passport.js and return a JWT token upon successful authentication.

---

## 12. Social Sharing

### GET /api/share/property
- **Description:** Generates a shareable URL for a property.
- **Query Parameter:** `propertyId`
- **Usage Scenario:** Allow users to share property details on social media.
- **Sample Request:**  
  `GET /api/share/property?propertyId=1`
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/properties/1" }
  ```

### GET /api/share/comment
- **Description:** Generates a shareable URL for a comment.
- **Query Parameter:** `commentId`
- **Usage Scenario:** Allow users to share property comments on social media.
- **Sample Request:**  
  `GET /api/share/comment?commentId=1`
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/comments/1" }
  ```

---

## 13. Real-time Notifications & Chat

### Notifications

#### POST /api/notifications/send
- **Description:** Sends a real-time push notification to a specific room via Socket.IO.
- **Request Body:** (JSON)
  ```json
  {
    "room": "user_1",
    "notification": "رزرو شما تایید شد!"
  }
  ```
- **Usage Scenario:** Inform users instantly about booking confirmation, payment updates, etc.
- **Sample Response:**
  ```json
  { "message": "Notification sent successfully" }
  ```

### Chat

#### GET /api/chats/:room
- **Description:** Retrieves chat history for a specified room.
- **URL Parameter:** `room`
- **Usage Scenario:** Display chat conversation in a specific room.
- **Sample Request:**  
  `GET /api/chats/room123`
- **Sample Response:**
  ```json
  [
    { "id": 1, "room": "room123", "sender": "user_1", "message": "سلام!", "createdAt": "2023-03-01T00:00:00.000Z" },
    ...
  ]
  ```

#### POST /api/chats/send
- **Description:** Sends a chat message to a room via Socket.IO.
- **Request Body:** (JSON)
  ```json
  {
    "room": "room123",
    "sender": "user_1",
    "message": "سلام، چگونه می‌توانم کمکتان کنم؟"
  }
  ```
- **Usage Scenario:** Users communicate in real-time.
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

## 14. Advanced Reporting (Dashboard)

### GET /api/reporting/advanced
- **Description:** Retrieves advanced analytical reports including:
  - **Conversion Rate:** Percentage of bookings with transaction type "direct_purchase".
  - **Average Booking Price:** Average price of booked houses.
  - **Total Revenue:** Sum of prices for booked houses.
  - **Booking Distribution:** Count of bookings grouped by transaction type.
  - **Heatmap Data:** Number of houses per location (grouped by area_name).
- **Usage Scenario:** Admins view comprehensive performance metrics.
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

## 15. User Profile Management

### PUT /api/users/profile/:id
- **Description:** Updates a user's public profile with additional details.
- **URL Parameter:** `id`
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
- **Usage Scenario:** Users update their personal and public profile information.
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

## 16. Recommendation System

### GET /api/recommendations/:userId
- **Description:** Retrieves property recommendations for a user based on historical data and behavior.
- **URL Parameter:** `userId`
- **Usage Scenario:** Provides intelligent suggestions for properties (dummy implementation returns top 5 cheapest houses).
- **Sample Response:**
  ```json
  {
    "userId": 5,
    "recommendations": [
      { "id": 3, "title": "خانه اقتصادی", "price": 900000 },
      ...
    ]
  }
  ```

### POST /api/recommendations/predict
- **Description:** Predicts the price of a property based on its features.
- **Request Body:** (JSON)
  ```json
  { "size": 120, "rooms": 3 }
  ```
- **Usage Scenario:** Provides a price prediction to help users evaluate property costs.
- **Sample Response:**
  ```json
  { "predictedPrice": 190000 }
  ```

---

## 17. Q&A for Properties

### POST /api/property-qas/question
- **Description:** Creates a new question for a property.
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 1,
    "question": "این ملک دارای امکانات رفاهی کامل است؟"
  }
  ```
- **Usage Scenario:** Users ask questions about property details.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "houseId": 1,
    "userId": 2,
    "question": "این ملک دارای امکانات رفاهی کامل است؟",
    "createdAt": "2023-03-01T00:00:00.000Z"
  }
  ```

### POST /api/property-qas/answer
- **Description:** Posts an answer to an existing question.
- **Request Body:** (JSON)
  ```json
  {
    "questionId": 1,
    "answer": "بله، این ملک دارای امکانات کامل از جمله پارکینگ و آسانسور است."
  }
  ```
- **Usage Scenario:** Sellers or other users answer property-related questions.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "houseId": 1,
    "userId": 2,
    "question": "این ملک دارای امکانات رفاهی کامل است؟",
    "answer": "بله، این ملک دارای امکانات کامل از جمله پارکینگ و آسانسور است.",
    "answeredBy": 10,
    "updatedAt": "2023-03-02T00:00:00.000Z"
  }
  ```

### GET /api/property-qas/:houseId
- **Description:** Retrieves all Q&A for a specific property.
- **URL Parameter:** `houseId`
- **Usage Scenario:** Users view all questions and answers for a property.
- **Sample Response:**
  ```json
  [
    {
      "id": 1,
      "houseId": 1,
      "userId": 2,
      "question": "این ملک دارای امکانات رفاهی کامل است؟",
      "answer": "بله، این ملک دارای امکانات کامل از جمله پارکینگ و آسانسور است.",
      "createdAt": "2023-03-01T00:00:00.000Z"
    },
    ...
  ]
  ```

---

## 18. Price History Tracking

### GET /api/price-history/:houseId
- **Description:** Retrieves the full price history for a property.
- **URL Parameter:** `houseId`
- **Usage Scenario:** Users view historical price data for a property.
- **Sample Response:**
  ```json
  [
    { "id": 1, "houseId": 1, "price": 1200000, "recorded_at": "2023-01-01T00:00:00.000Z" },
    { "id": 2, "houseId": 1, "price": 1150000, "recorded_at": "2023-02-01T00:00:00.000Z" }
  ]
  ```

### GET /api/price-history/:houseId/monthly-changes
- **Description:** Retrieves monthly percentage changes in price for a property.
- **URL Parameter:** `houseId`
- **Usage Scenario:** Users analyze price trends.
- **Sample Response:**
  ```json
  [
    { "month": "2023-01", "percentageChange": "0.00" },
    { "month": "2023-02", "percentageChange": "-4.17" }
  ]
  ```

---

## 19. Property Comparison

### GET /api/comparison
- **Description:** Retrieves comparison data for multiple properties based on provided IDs.
- **Query Parameter:** `ids` (Comma-separated list of property IDs)
- **Usage Scenario:** Users compare selected properties side by side.
- **Sample Request:**  
  `GET /api/comparison?ids=1,2,3`
- **Sample Response:**
  ```json
  [
    { "id": 1, "title": "آپارتمان مدرن", "price": 1200000, "capacity": 4, "rate": 4.5 },
    { "id": 2, "title": "خانه روستایی دنج", "price": 1500000, "capacity": 3, "rate": 4.7 },
    { "id": 3, "title": "ویلای لوکس", "price": 5000000, "capacity": 8, "rate": 4.9 }
  ]
  ```

---

## 20. Saved Searches

### POST /api/saved-searches
- **Description:** Saves a search query for a user.
- **Request Body:** (JSON)
  ```json
  {
    "userId": 1,
    "searchQuery": "املاک اجاره‌ای در تهران",
    "note": "برای لیست خرید آینده"
  }
  ```
- **Usage Scenario:** Users save search queries for later reference.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "userId": 1,
    "searchQuery": "املاک اجاره‌ای در تهران",
    "note": "برای لیست خرید آینده",
    "createdAt": "2023-04-01T00:00:00.000Z"
  }
  ```

### GET /api/saved-searches/:userId
- **Description:** Retrieves all saved searches for a user.
- **URL Parameter:** `userId`
- **Usage Scenario:** Display user's saved search history.
- **Sample Response:**
  ```json
  [
    { "id": 1, "userId": 1, "searchQuery": "املاک اجاره‌ای در تهران", "note": "برای لیست خرید آینده", "createdAt": "2023-04-01T00:00:00.000Z" },
    ...
  ]
  ```

### DELETE /api/saved-searches/:id
- **Description:** Deletes a saved search entry.
- **URL Parameter:** `id`
- **Usage Scenario:** Users remove outdated saved searches.
- **Sample Response:**
  ```json
  { "message": "Saved search deleted successfully" }
  ```

---

## 21. Wishlists

### POST /api/wishlists
- **Description:** Adds a property to the user's wishlist with an optional note.
- **Request Body:** (JSON)
  ```json
  {
    "userId": 1,
    "houseId": 2,
    "note": "خانه با موقعیت عالی، مناسب برای سرمایه‌گذاری"
  }
  ```
- **Usage Scenario:** Users bookmark properties they are interested in and add personal notes.
- **Sample Response:**
  ```json
  { "id": 1, "userId": 1, "houseId": 2, "note": "خانه با موقعیت عالی، مناسب برای سرمایه‌گذاری", "createdAt": "2023-04-01T00:00:00.000Z" }
  ```

### GET /api/wishlists/:userId
- **Description:** Retrieves the wishlist of a specific user.
- **URL Parameter:** `userId`
- **Usage Scenario:** Display user's favorite properties.
- **Sample Response:**
  ```json
  [
    { "id": 1, "userId": 1, "houseId": 2, "note": "خانه با موقعیت عالی، مناسب برای سرمایه‌گذاری", "createdAt": "2023-04-01T00:00:00.000Z" },
    ...
  ]
  ```

### DELETE /api/wishlists
- **Description:** Removes an item from the wishlist.
- **Request Body:** (JSON)
  ```json
  { "userId": 1, "houseId": 2 }
  ```
- **Usage Scenario:** Users remove properties from their wishlist.
- **Sample Response:**
  ```json
  { "message": "Wishlist item removed successfully" }
  ```

---

## 22. Property Availability Calendar

### GET /api/houses/:id/availability
- **Description:** Retrieves available dates for a property in the next 30 days by excluding dates with existing bookings.
- **URL Parameter:** `id` (House identifier)
- **Usage Scenario:** Users view a calendar of available dates for property visits or bookings.
- **Sample Response:**
  ```json
  {
    "houseId": 1,
    "availableDates": ["2023-05-01", "2023-05-02", "2023-05-04", ...]
  }
  ```

---

## 23. Seller Upgrade

### POST /api/users/upgrade-to-seller
- **Description:** Initiates the upgrade process for a user to become a seller by creating a payment request.
- **Usage Scenario:** A user wishing to become a seller must pay a fee.
- **Sample Response:**
  ```json
  {
    "message": "Upgrade payment initiated",
    "payment": {
      "authority": "FAKE_AUTH_CODE_ABC123",
      "paymentUrl": "https://payment-gateway.example.com/pay/FAKE_AUTH_CODE_ABC123"
    }
  }
  ```

### POST /api/users/upgrade-to-seller/verify
- **Description:** Verifies the seller upgrade payment and updates the user's role to "seller".
- **Request Body:** (JSON)
  ```json
  { "authority": "FAKE_AUTH_CODE_ABC123", "status": "OK" }
  ```
- **Usage Scenario:** After successful payment, the user's role is updated.
- **Sample Response:**
  ```json
  { "message": "User upgraded to seller successfully" }
  ```

---

## 24. Social Bookmark with Notes

### POST /api/share/bookmark
- **Description:** Saves a property to the user's wishlist with an additional personal note and generates a shareable URL.
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 2,
    "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است."
  }
  ```
- **Usage Scenario:** Users bookmark a property and add a personal note, then share the bookmark URL.
- **Sample Response:**
  ```json
  {
    "bookmark": {
      "id": 1,
      "userId": 1,
      "houseId": 2,
      "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است.",
      "createdAt": "2023-04-01T00:00:00.000Z"
    },
    "shareUrl": "https://yourdomain.com/properties/2?bookmarkNote=%D8%A7%DB%8C%D9%86%20%D9%85%D9%84%DA%A9"
  }
  ```

---

## 25. Targeted Notifications

### POST /api/notifications/settings
- **Description:** Creates a new targeted notification setting for a user.
- **Request Body:** (JSON)
  ```json
  {
    "notificationType": "price_drop",
    "criteria": { "priceThreshold": 1000000 }
  }
  ```
- **Usage Scenario:** A user sets up notifications to be alerted when a property in their interest area drops below a certain price.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "userId": 1,
    "notificationType": "price_drop",
    "criteria": { "priceThreshold": 1000000 },
    "createdAt": "2023-04-01T00:00:00.000Z"
  }
  ```

### GET /api/notifications/settings
- **Description:** Retrieves all targeted notification settings for the authenticated user.
- **Usage Scenario:** Display the user’s current notification preferences.
- **Sample Response:**
  ```json
  [
    {
      "id": 1,
      "userId": 1,
      "notificationType": "price_drop",
      "criteria": { "priceThreshold": 1000000 },
      "createdAt": "2023-04-01T00:00:00.000Z"
    }
  ]
  ```

### PUT /api/notifications/settings/:id
- **Description:** Updates an existing targeted notification setting.
- **URL Parameter:** `id` (Setting identifier)
- **Request Body:** (JSON)
  ```json
  { "criteria": { "priceThreshold": 900000 } }
  ```
- **Sample Response:**
  ```json
  {
    "id": 1,
    "userId": 1,
    "notificationType": "price_drop",
    "criteria": { "priceThreshold": 900000 },
    "updatedAt": "2023-04-02T00:00:00.000Z"
  }
  ```

### DELETE /api/notifications/settings/:id
- **Description:** Deletes a targeted notification setting.
- **URL Parameter:** `id`
- **Sample Response:**
  ```json
  { "message": "Notification setting deleted successfully" }
  ```

### POST /api/notifications/send-scheduled
- **Description:** Simulates sending scheduled targeted notifications.
- **Usage Scenario:** This endpoint can be triggered by a background job to send notifications based on the saved settings.
- **Sample Response:**
  ```json
  { "message": "Scheduled notifications sent", "settings": [ /* list of settings */ ] }
  ```

---

## Scenarios Overview

1. **User Registration & Login:**  
   A new user registers with default role "buyer" and logs in to receive JWT tokens. Only authorized users (with valid tokens) can access restricted endpoints.

2. **Property Browsing & Advanced Search:**  
   Users browse properties using various filters, sort by rating, or search within a geographical area.

3. **Booking & Payment:**  
   Users reserve properties, initiate payments, and verify transactions. Payment details are handled securely and update booking status.

4. **Feedback & Loyalty:**  
   After a transaction, users leave feedback with ratings. The system calculates and aggregates loyalty points based on feedback.

5. **Property Recommendations & Price Prediction:**  
   The recommendation system suggests properties to users and provides price estimates based on property features.

6. **Q&A for Properties:**  
   Users can ask questions about a property and receive answers from sellers or other users.

7. **Price History Tracking:**  
   Users view a property’s historical price data along with monthly percentage changes to understand market trends.

8. **Property Comparison:**  
   Users select multiple properties to compare their features, prices, and other details side by side.

9. **Saved Searches & Wishlists:**  
   Users save their search queries and bookmark properties with personal notes for future reference.

10. **Property Availability Calendar:**  
    Users view a calendar showing available dates for property bookings or visits.

11. **Seller Upgrade:**  
    Users who wish to become sellers must pay an upgrade fee. Upon successful payment verification, their role is updated.

12. **Social Bookmark with Notes:**  
    Users can bookmark properties along with personal notes and share a generated URL on social media.

13. **Targeted Notifications:**  
    Users set up notification settings (e.g., for price drops or new properties) and receive scheduled alerts when criteria are met.

14. **Real-time Communication:**  
    The API supports real-time notifications and chat functionalities to facilitate instant user interactions.

15. **Advanced Reporting:**  
    Administrators access detailed analytical reports on bookings, revenue, and property distribution.

16. **User Profile Management:**  
    Users update and manage their public profiles, including contact information and transaction history.

---

This documentation covers all API endpoints along with their parameters, sample requests/responses, and usage scenarios. It provides a complete reference for frontend developers to understand and integrate with the Real Estate API.
