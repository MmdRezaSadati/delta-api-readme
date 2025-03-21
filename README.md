# Real Estate API Documentation

## Introduction

This API provides a comprehensive suite of endpoints to manage a complete real estate platform. It covers core functionalities such as property management, user accounts, bookings, payments, feedback, Q&A, price history, and social authentication/sharing, as well as additional value-added features like blogs, recommendations, reporting, advanced search, notifications, and more.

**Total Endpoints:** Approximately 78 endpoints are available.  
These endpoints are organized into various categories to support all the features of the platform, ensuring that both essential and additional functionalities are fully covered.

> **Note:** This documentation details each API endpoint, including required parameters, headers (if applicable), sample requests/responses, and usage scenarios. It is intended for frontend developers integrating with the API.

---

## Table of Contents

1. [Properties (Houses)](#properties-houses)
2. [Users](#users)
3. [Bookings](#bookings)
4. [Payment & Reservations](#payment--reservations)
5. [Feedback, Ratings & Q&A](#feedback-ratings--qa)
6. [Price History Tracking](#price-history-tracking)
7. [Social Authentication & Sharing](#social-authentication--sharing)
8. [Notifications](#notifications)
9. [Recommendation System](#recommendation-system)
10. [Blogs & Categories](#blogs--categories)
11. [Favorites, Saved Searches & Wishlists](#favorites-saved-searches--wishlists)
12. [Virtual Open House Scheduling](#virtual-open-house-scheduling)
13. [Seller Upgrade](#seller-upgrade)
14. [Social Bookmark with Notes](#social-bookmark-with-notes)
15. [User Activity Dashboard](#user-activity-dashboard)

---

## 1. Properties (Houses)

### GET /api/houses
- **Description:** Retrieves all properties with filtering, pagination, and sorting.
- **Query Parameters:**
  - `page` (optional, default: "1")
  - `limit` (optional, default: "10")
  - `sort` (optional): Field to sort by (e.g., price, rate)
  - `order` (optional, default: "ASC"): Sort order ("ASC" or "DESC")
  - Additional filters: `title`, `address`, `rate`, `price`, `capacity`, `transaction_type`
- **Headers:** None required.
- **Usage Scenario:** Users browse the property listings using various filters.
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
- **Description:** Retrieves detailed information for a specific property.
- **URL Parameter:** `id` – the property identifier.
- **Usage Scenario:** When a user selects a property for more details.
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
    "caption": "توضیحات کامل درباره ملک",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### POST /api/houses
- **Description:** Creates a new property. The seller details (sellerId, sellerName, sellerPicture) are automatically extracted from the authenticated user's token.
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
    "categories": { "categories": ["مسکونی"] },
    "caption": "توضیحات کامل ملک"
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Sellers create new property listings.
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
- **Description:** Updates a property's information.
- **URL Parameter:** `id` – the property identifier.
- **Request Body:** (JSON)
  ```json
  {
    "price": 1300000,
    "capacity": 5
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>` (Seller-only)
- **Usage Scenario:** Sellers update their property details.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "price": 1300000,
    "capacity": 5,
    "updatedAt": "2023-02-01T00:00:00.000Z"
  }
  ```

### DELETE /api/houses/:id
- **Description:** Deletes a property.
- **URL Parameter:** `id`
- **Headers:**  
  - `Authorization: Bearer <token>` (Seller-only)
- **Usage Scenario:** Sellers remove properties that are no longer available.
- **Sample Response:**
  ```json
  { "message": "House deleted successfully" }
  ```

### GET /api/houses/by-rating
- **Description:** Retrieves properties sorted by average rating.
- **Usage Scenario:** Display properties prioritized by user reviews.
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
- **Description:** Retrieves properties within a specified geographical radius.
- **Query Parameters:**
  - `lat`: Latitude (required)
  - `lng`: Longitude (required)
  - `radius`: Radius in meters (required)
- **Usage Scenario:** Users search for properties near a specific location.
- **Sample Request:**  
  `GET /api/houses/geo-search?lat=35.6892&lng=51.3890&radius=5000`
- **Sample Response:**
  ```json
  [
    { "id": 3, "title": "خانه در مرکز شهر", ... },
    ...
  ]
  ```

### GET /api/houses/:id/availability
- **Description:** Retrieves available dates for a property in the next 30 days by excluding dates with existing bookings.
- **URL Parameter:** `id`
- **Usage Scenario:** Users check availability for property visits or bookings.
- **Sample Response:**
  ```json
  {
    "houseId": 1,
    "availableDates": ["2023-05-01", "2023-05-02", "2023-05-04", ...]
  }
  ```

---

## 2. Users

### POST /api/auth/register
- **Description:** Registers a new user with required fields. The role is hard-coded to "buyer".
- **Request Body:** (JSON)
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword",
    "firstName": "علی",
    "lastName": "کاربر"
  }
  ```
- **Usage Scenario:** New users sign up.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "fullName": "علی کاربر",
    "firstName": "علی",
    "lastName": "کاربر",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
  ```

### POST /api/auth/login
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
- **Usage Scenario:** Obtain a new access token.
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
- **Description:** Retrieves all users. (Admin-only)
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Usage Scenario:** Admin user management.
- **Sample Response:**
  ```json
  [
    { "id": 1, "email": "user@example.com", "role": "buyer", "fullName": "علی کاربر" },
    ...
  ]
  ```

### GET /api/users/:id
- **Description:** Retrieves detailed information of a specific user.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** View full user profile.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "email": "user@example.com",
    "role": "buyer",
    "fullName": "علی کاربر",
    "firstName": "علی",
    "lastName": "کاربر",
    "profile_picture": "default_profile.jpg",
    "membership_date": "2023-01-01T00:00:00.000Z"
  }
  ```

### PUT /api/users/:id
- **Description:** Updates a user's information (e.g., name, password).
- **Request Body:** (JSON)
  ```json
  { "fullName": "علی کاربر به‌روز شده", "password": "newSecurePassword" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User updates their account.
- **Sample Response:**
  ```json
  { "id": 1, "email": "user@example.com", "fullName": "علی کاربر به‌روز شده" }
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Public profile update.
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
- **Description:** Deletes a user. (Admin-only)
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Usage Scenario:** Remove user from the system.
- **Sample Response:**
  ```json
  { "message": "User deleted successfully" }
  ```

---

## 3. Bookings

### GET /api/bookings
- **Description:** Retrieves all bookings with filtering, pagination, and sorting.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** List all bookings for the authenticated user.
- **Sample Response:**
  ```json
  [
    { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "passengers": [...] } },
    ...
  ]
  ```

### GET /api/bookings/:id
- **Description:** Retrieves details of a specific booking.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** View detailed booking information.
- **Sample Response:**
  ```json
  { "id": 1, "house_id": 2, "reservedDates": ["2023-05-01", "2023-05-10"], "traveler_details": { "passengers": [...] } }
  ```

### POST /api/bookings
- **Description:** Creates a new booking with status "pending".  
  Traveler details must be provided as an array of passenger objects, each containing: firstName, lastName, gender, birthDate, and nationalId. Optionally, sharedEmail and sharedMobile can be provided for ticket sharing.
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": [
      {
        "firstName": "سارا",
        "lastName": "رضایی",
        "gender": "female",
        "birthDate": "1990-05-15",
        "nationalId": "1234567890"
      },
      {
        "firstName": "علی",
        "lastName": "کاظمی",
        "gender": "male",
        "birthDate": "1988-10-20",
        "nationalId": "0987654321"
      }
    ],
    "sharedEmail": "friend@example.com",
    "sharedMobile": "09121234567"
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User creates a booking and optionally shares the ticket with someone else.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "houseId": 2,
    "reservedDates": ["2023-05-01", "2023-05-10"],
    "traveler_details": [ ... ],
    "status": "pending",
    "sharedEmail": "friend@example.com",
    "sharedMobile": "09121234567"
  }
  ```

### PUT /api/bookings/:id
- **Description:** Updates an existing booking.
- **Request Body:** (JSON)
  ```json
  { "reservedDates": ["2023-05-02", "2023-05-12"] }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User updates their reservation details.
- **Sample Response:**
  ```json
  { "id": 1, "houseId": 2, "reservedDates": ["2023-05-02", "2023-05-12"] }
  ```

### DELETE /api/bookings/:id
- **Description:** Deletes a booking.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Cancel a booking.
- **Sample Response:**
  ```json
  { "message": "Booking deleted successfully" }
  ```

### POST /api/bookings/:id/cancel
- **Description:** Cancels a pending booking (sets status to "canceled").
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User cancels a booking before payment is completed.
- **Sample Response:**
  ```json
  { "message": "Booking canceled successfully", "booking": { ... } }
  ```

### POST /api/bookings/:id/continue
- **Description:** Continues a canceled booking (sets status back to "pending").
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User decides to proceed with a previously canceled booking.
- **Sample Response:**
  ```json
  { "message": "Booking continued successfully", "booking": { ... } }
  ```

---

## 4. Payment & Reservations

### POST /api/payments/checkout
- **Description:** Initiates a payment process for a booking.
- **Request Body:** (JSON)
  ```json
  { "bookingId": 1, "amount": 1200000 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User starts the payment process.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Verify the completion of a payment.
- **Sample Response:**
  ```json
  { "status": "paid" }
  ```

### POST /api/payments/verify
- **Description:** Verifies a payment and confirms the booking.
- **Request Body:** (JSON)
  ```json
  { "authority": "FAKE_AUTH_CODE_12345", "status": "OK", "bookingId": 1 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** After payment, the booking is confirmed and ticket is sent.
- **Sample Response:**
  ```json
  { "message": "Payment verified and booking confirmed" }
  ```

---

## 5. Feedback, Ratings & Q&A

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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User leaves feedback after a transaction.
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
- **Description:** Retrieves all feedback received by a user along with total loyalty points.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Display feedback summary on user dashboard.
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
- **Description:** Retrieves all feedback submitted by a user.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** View reviews that a user has written.
- **Sample Response:**
  ```json
  [
    { "id": 1, "rating": 4.5, "comment": "معامله عالی بود!", "pointsAwarded": 10 },
    ...
  ]
  ```

### GET /api/feedback/loyalty/:userId
- **Description:** Retrieves the total loyalty points accumulated by a user.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Show user's overall loyalty score.
- **Sample Response:**
  ```json
  { "userId": 3, "totalPoints": 25 }
  ```

### POST /api/property-qas/question
- **Description:** Creates a new question for a property.
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 1,
    "question": "این ملک دارای امکانات رفاهی کامل است؟"
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Users ask questions regarding property details.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Sellers or other users respond to questions.
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

## 6. Price History Tracking

### GET /api/price-history/:houseId
- **Description:** Retrieves the complete price history for a property.
- **URL Parameter:** `houseId`
- **Usage Scenario:** Users view historical price data.
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
- **Usage Scenario:** Users analyze price trends over time.
- **Sample Response:**
  ```json
  [
    { "month": "2023-01", "percentageChange": "0.00" },
    { "month": "2023-02", "percentageChange": "-4.17" }
  ]
  ```

---

## 7. Social Authentication & Sharing

### Social Authentication Endpoints
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

### GET /api/share/property
- **Description:** Generates a shareable URL for a property.
- **Query Parameter:** `propertyId`
- **Usage Scenario:** Users share property details on social media.
- **Sample Request:**  
  `GET /api/share/property?propertyId=1`
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/properties/1" }
  ```

### GET /api/share/comment
- **Description:** Generates a shareable URL for a comment.
- **Query Parameter:** `commentId`
- **Usage Scenario:** Users share comments on social media.
- **Sample Request:**  
  `GET /api/share/comment?commentId=1`
- **Sample Response:**
  ```json
  { "shareUrl": "https://yourdomain.com/comments/1" }
  ```

---

## 8. Notifications

### POST /api/notifications/send
- **Description:** Sends a real-time push notification via Socket.IO.
- **Request Body:** (JSON)
  ```json
  {
    "room": "user_1",
    "notification": "رزرو شما تایید شد!"
  }
  ```
- **Usage Scenario:** Inform users instantly (e.g., booking confirmation).
- **Sample Response:**
  ```json
  { "message": "Notification sent successfully" }
  ```

### POST /api/notifications/settings
- **Description:** Creates a new targeted notification setting.
- **Request Body:** (JSON)
  ```json
  {
    "notificationType": "price_drop",
    "criteria": { "priceThreshold": 1000000 }
  }
  ```
- **Usage Scenario:** Users set preferences for notifications.
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
- **Usage Scenario:** Display user notification preferences.
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
- **Description:** Updates an existing notification setting.
- **URL Parameter:** `id`
- **Request Body:** (JSON)
  ```json
  { "criteria": { "priceThreshold": 900000 } }
  ```
- **Usage Scenario:** Modify notification preferences.
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
- **Usage Scenario:** Remove a notification preference.
- **Sample Response:**
  ```json
  { "message": "Notification setting deleted successfully" }
  ```

### POST /api/notifications/send-scheduled
- **Description:** Simulates sending scheduled notifications.
- **Usage Scenario:** This endpoint can be triggered by a background job.
- **Sample Response:**
  ```json
  { "message": "Scheduled notifications sent", "settings": [ /* list of settings */ ] }
  ```

---

## 9. Recommendation System

### GET /api/recommendations/:userId
- **Description:** Retrieves property recommendations for a user based on historical data.
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Intelligent property suggestions.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Provide estimated property price.
- **Sample Response:**
  ```json
  { "predictedPrice": 190000 }
  ```

---

## 10. Blogs & Categories

### Blogs

#### GET /api/blogs
- **Description:** Retrieves all blog posts.
- **Usage Scenario:** Display blog content.
- **Sample Response:**
  ```json
  [
    { "id": 1, "title": "روندهای املاک", "author_id": 2, "category_id": 3, "createdAt": "2023-01-01T00:00:00.000Z" },
    ...
  ]
  ```

#### GET /api/blogs/:id
- **Description:** Retrieves a specific blog post.
- **Usage Scenario:** View detailed blog content.
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3 }
  ```

#### POST /api/blogs
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
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin or Seller)
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای جدید در املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3, "createdAt": "2023-03-01T00:00:00.000Z" }
  ```

#### PUT /api/blogs/:id
- **Description:** Updates an existing blog post.
- **Request Body:** (JSON)
  ```json
  { "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده..." }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin or Seller)
- **Sample Response:**
  ```json
  { "id": 1, "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده...", "updatedAt": "2023-03-05T00:00:00.000Z" }
  ```

#### DELETE /api/blogs/:id
- **Description:** Deletes a blog post.
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Usage Scenario:** Remove outdated blog posts.
- **Sample Response:**
  ```json
  { "message": "Blog post deleted successfully" }
  ```

### Categories

#### GET /api/categories
- **Description:** Retrieves all categories.
- **Sample Response:**
  ```json
  [
    { "id": 1, "name": "مسکونی" },
    { "id": 2, "name": "تجاری" }
  ]
  ```

#### GET /api/categories/:id
- **Description:** Retrieves details of a specific category.
- **Sample Response:**
  ```json
  { "id": 1, "name": "مسکونی" }
  ```

#### POST /api/categories
- **Description:** Creates a new category.
- **Request Body:** (JSON)
  ```json
  { "name": "لوکس" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Sample Response:**
  ```json
  { "id": 3, "name": "لوکس" }
  ```

#### PUT /api/categories/:id
- **Description:** Updates an existing category.
- **Request Body:** (JSON)
  ```json
  { "name": "دسته‌بندی بروزرسانی شده" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Sample Response:**
  ```json
  { "id": 1, "name": "دسته‌بندی بروزرسانی شده" }
  ```

#### DELETE /api/categories/:id
- **Description:** Deletes a category.
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Sample Response:**
  ```json
  { "message": "Category deleted successfully" }
  ```

---

## 11. Favorites, Saved Searches & Wishlists

### Favorites

#### POST /api/favorites/add
- **Description:** Adds a house to a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "id": 1, "user_id": 1, "house_id": 2 }
  ```

#### DELETE /api/favorites/remove
- **Description:** Removes a house from a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Favorite removed successfully" }
  ```

#### GET /api/favorites/:user_id
- **Description:** Retrieves all favorite houses for a specific user.
- **URL Parameter:** `user_id`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Request:**  
  `GET /api/favorites/1?page=1&limit=10`
- **Sample Response:**
  ```json
  [
    { "id": 1, "user_id": 1, "house_id": 2 },
    ...
  ]
  ```

### Saved Searches

#### POST /api/saved-searches
- **Description:** Saves a search query for a user.
- **Request Body:** (JSON)
  ```json
  {
    "userId": 1,
    "searchQuery": "املاک اجاره‌ای در تهران",
    "note": "برای لیست خرید آینده"
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
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

#### GET /api/saved-searches/:userId
- **Description:** Retrieves all saved searches for a user.
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    {
      "id": 1,
      "userId": 1,
      "searchQuery": "املاک اجاره‌ای در تهران",
      "note": "برای لیست خرید آینده",
      "createdAt": "2023-04-01T00:00:00.000Z"
    },
    ...
  ]
  ```

#### DELETE /api/saved-searches/:id
- **Description:** Deletes a saved search entry.
- **URL Parameter:** `id`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Saved search deleted successfully" }
  ```

### Wishlists

#### POST /api/wishlists
- **Description:** Adds a property to the user's wishlist with an optional note.
- **Request Body:** (JSON)
  ```json
  {
    "userId": 1,
    "houseId": 2,
    "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است."
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  {
    "id": 1,
    "userId": 1,
    "houseId": 2,
    "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است.",
    "createdAt": "2023-04-01T00:00:00.000Z"
  }
  ```

#### GET /api/wishlists/:userId
- **Description:** Retrieves the wishlist for a user.
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    {
      "id": 1,
      "userId": 1,
      "houseId": 2,
      "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است.",
      "createdAt": "2023-04-01T00:00:00.000Z"
    },
    ...
  ]
  ```

#### DELETE /api/wishlists
- **Description:** Removes an item from the wishlist.
- **Request Body:** (JSON)
  ```json
  { "userId": 1, "houseId": 2 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Wishlist item removed successfully" }
  ```

---

## 12. Virtual Open House Scheduling

### POST /api/appointments
- **Description:** Creates a new appointment for a property visit (virtual or in-person).
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 1,
    "appointmentTime": "2023-06-15T14:30:00.000Z",
    "type": "virtual"
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Users schedule a property visit.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "houseId": 1,
    "userId": 5,
    "appointmentTime": "2023-06-15T14:30:00.000Z",
    "type": "virtual",
    "status": "pending",
    "createdAt": "2023-05-01T12:00:00.000Z"
  }
  ```

### GET /api/appointments/house/:houseId
- **Description:** Retrieves all appointments for a specific property.
- **URL Parameter:** `houseId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    { "id": 1, "houseId": 1, "appointmentTime": "2023-06-15T14:30:00.000Z", "type": "virtual", "status": "pending" },
    ...
  ]
  ```

### GET /api/appointments/user
- **Description:** Retrieves all appointments for the authenticated user.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    { "id": 1, "houseId": 1, "appointmentTime": "2023-06-15T14:30:00.000Z", "type": "virtual", "status": "pending" },
    ...
  ]
  ```

### PUT /api/appointments/:id
- **Description:** Updates an existing appointment (e.g., change appointment time or type).
- **URL Parameter:** `id`
- **Request Body:** (JSON)
  ```json
  { "appointmentTime": "2023-06-16T15:00:00.000Z", "type": "in_person" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "id": 1, "houseId": 1, "appointmentTime": "2023-06-16T15:00:00.000Z", "type": "in_person", "status": "pending" }
  ```

### DELETE /api/appointments/:id
- **Description:** Deletes an appointment.
- **URL Parameter:** `id`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Appointment deleted successfully" }
  ```

---

## 13. Seller Upgrade

### POST /api/users/upgrade-to-seller
- **Description:** Initiates the upgrade process for a user to become a seller by creating a payment request.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User initiates seller upgrade (payment required).
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** After successful payment, user role is updated.
- **Sample Response:**
  ```json
  { "message": "User upgraded to seller successfully" }
  ```

---

## 14. Social Bookmark with Notes

### POST /api/share/bookmark
- **Description:** Saves a property to the user's wishlist with a personal note and generates a shareable URL.
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 2,
    "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است."
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Users bookmark properties with personal notes for sharing.
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

## 15. Targeted Notifications

### POST /api/notifications/settings
- **Description:** Creates a new targeted notification setting for a user.
- **Request Body:** (JSON)
  ```json
  {
    "notificationType": "price_drop",
    "criteria": { "priceThreshold": 1000000 }
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Users set preferences for receiving notifications.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Display current notification preferences.
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
- **URL Parameter:** `id`
- **Request Body:** (JSON)
  ```json
  { "criteria": { "priceThreshold": 900000 } }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Modify notification settings.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Remove a notification setting.
- **Sample Response:**
  ```json
  { "message": "Notification setting deleted successfully" }
  ```

### POST /api/notifications/send-scheduled
- **Description:** Simulates sending scheduled notifications based on saved settings.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** This endpoint may be triggered by a background job.
- **Sample Response:**
  ```json
  { "message": "Scheduled notifications sent", "settings": [ /* list of settings */ ] }
  ```

---

## 16. Recommendation System

### GET /api/recommendations/:userId
- **Description:** Retrieves property recommendations for a user based on historical data.
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Intelligent suggestions for properties.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Provide an estimated property price.
- **Sample Response:**
  ```json
  { "predictedPrice": 190000 }
  ```

---

## 17. Blogs & Categories

### Blogs

#### GET /api/blogs
- **Description:** Retrieves all blog posts.
- **Usage Scenario:** Display blog content.
- **Sample Response:**
  ```json
  [
    { "id": 1, "title": "روندهای املاک", "author_id": 2, "category_id": 3, "createdAt": "2023-01-01T00:00:00.000Z" },
    ...
  ]
  ```

#### GET /api/blogs/:id
- **Description:** Retrieves a specific blog post.
- **Usage Scenario:** View detailed blog content.
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3 }
  ```

#### POST /api/blogs
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
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin or Seller)
- **Usage Scenario:** Content creation.
- **Sample Response:**
  ```json
  { "id": 1, "title": "روندهای جدید در املاک", "content": "متن مقاله...", "author_id": 2, "category_id": 3, "createdAt": "2023-03-01T00:00:00.000Z" }
  ```

#### PUT /api/blogs/:id
- **Description:** Updates an existing blog post.
- **Request Body:** (JSON)
  ```json
  { "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده..." }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin or Seller)
- **Usage Scenario:** Editing blog posts.
- **Sample Response:**
  ```json
  { "id": 1, "title": "بروزرسانی روندهای املاک", "content": "متن بروزرسانی شده...", "updatedAt": "2023-03-05T00:00:00.000Z" }
  ```

#### DELETE /api/blogs/:id
- **Description:** Deletes a blog post.
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Usage Scenario:** Remove outdated content.
- **Sample Response:**
  ```json
  { "message": "Blog post deleted successfully" }
  ```

### Categories

#### GET /api/categories
- **Description:** Retrieves all categories.
- **Usage Scenario:** Display category list.
- **Sample Response:**
  ```json
  [
    { "id": 1, "name": "مسکونی" },
    { "id": 2, "name": "تجاری" }
  ]
  ```

#### GET /api/categories/:id
- **Description:** Retrieves details of a specific category.
- **Sample Response:**
  ```json
  { "id": 1, "name": "مسکونی" }
  ```

#### POST /api/categories
- **Description:** Creates a new category.
- **Request Body:** (JSON)
  ```json
  { "name": "لوکس" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Sample Response:**
  ```json
  { "id": 3, "name": "لوکس" }
  ```

#### PUT /api/categories/:id
- **Description:** Updates an existing category.
- **Request Body:** (JSON)
  ```json
  { "name": "دسته‌بندی بروزرسانی شده" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Sample Response:**
  ```json
  { "id": 1, "name": "دسته‌بندی بروزرسانی شده" }
  ```

#### DELETE /api/categories/:id
- **Description:** Deletes a category.
- **Headers:**  
  - `Authorization: Bearer <token>` (Admin)
- **Sample Response:**
  ```json
  { "message": "Category deleted successfully" }
  ```

---

## 18. Favorites, Saved Searches & Wishlists

### Favorites
#### POST /api/favorites/add
- **Description:** Adds a property to a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "id": 1, "user_id": 1, "house_id": 2 }
  ```

#### DELETE /api/favorites/remove
- **Description:** Removes a property from a user's favorites.
- **Request Body:** (JSON)
  ```json
  { "user_id": 1, "house_id": 2 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Favorite removed successfully" }
  ```

#### GET /api/favorites/:user_id
- **Description:** Retrieves all favorite properties for a specific user.
- **URL Parameter:** `user_id`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Request:**  
  `GET /api/favorites/1?page=1&limit=10`
- **Sample Response:**
  ```json
  [
    { "id": 1, "user_id": 1, "house_id": 2 },
    ...
  ]
  ```

### Saved Searches
#### POST /api/saved-searches
- **Description:** Saves a search query for a user.
- **Request Body:** (JSON)
  ```json
  {
    "userId": 1,
    "searchQuery": "املاک اجاره‌ای در تهران",
    "note": "برای لیست خرید آینده"
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
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

#### GET /api/saved-searches/:userId
- **Description:** Retrieves all saved searches for a user.
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    {
      "id": 1,
      "userId": 1,
      "searchQuery": "املاک اجاره‌ای در تهران",
      "note": "برای لیست خرید آینده",
      "createdAt": "2023-04-01T00:00:00.000Z"
    },
    ...
  ]
  ```

#### DELETE /api/saved-searches/:id
- **Description:** Deletes a saved search entry.
- **URL Parameter:** `id`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Saved search deleted successfully" }
  ```

### Wishlists
#### POST /api/wishlists
- **Description:** Adds a property to the user's wishlist with an optional note.
- **Request Body:** (JSON)
  ```json
  {
    "userId": 1,
    "houseId": 2,
    "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است."
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  {
    "id": 1,
    "userId": 1,
    "houseId": 2,
    "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است.",
    "createdAt": "2023-04-01T00:00:00.000Z"
  }
  ```

#### GET /api/wishlists/:userId
- **Description:** Retrieves the wishlist for a specific user.
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    {
      "id": 1,
      "userId": 1,
      "houseId": 2,
      "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است.",
      "createdAt": "2023-04-01T00:00:00.000Z"
    },
    ...
  ]
  ```

#### DELETE /api/wishlists
- **Description:** Removes an item from the wishlist using userId and houseId in the request body.
- **Request Body:** (JSON)
  ```json
  { "userId": 1, "houseId": 2 }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Wishlist item removed successfully" }
  ```

---

## 12. Virtual Open House Scheduling

### POST /api/appointments
- **Description:** Creates a new appointment for a property visit (virtual or in-person).
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 1,
    "appointmentTime": "2023-06-15T14:30:00.000Z",
    "type": "virtual"
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Users schedule a property visit.
- **Sample Response:**
  ```json
  {
    "id": 1,
    "houseId": 1,
    "userId": 5,
    "appointmentTime": "2023-06-15T14:30:00.000Z",
    "type": "virtual",
    "status": "pending",
    "createdAt": "2023-05-01T12:00:00.000Z"
  }
  ```

### GET /api/appointments/house/:houseId
- **Description:** Retrieves all appointments for a specific property.
- **URL Parameter:** `houseId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    { "id": 1, "houseId": 1, "appointmentTime": "2023-06-15T14:30:00.000Z", "type": "virtual", "status": "pending" },
    ...
  ]
  ```

### GET /api/appointments/user
- **Description:** Retrieves all appointments for the authenticated user.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  [
    { "id": 1, "houseId": 1, "appointmentTime": "2023-06-15T14:30:00.000Z", "type": "virtual", "status": "pending" },
    ...
  ]
  ```

### PUT /api/appointments/:id
- **Description:** Updates an existing appointment (e.g., change appointment time or type).
- **URL Parameter:** `id`
- **Request Body:** (JSON)
  ```json
  { "appointmentTime": "2023-06-16T15:00:00.000Z", "type": "in_person" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "id": 1, "houseId": 1, "appointmentTime": "2023-06-16T15:00:00.000Z", "type": "in_person", "status": "pending" }
  ```

### DELETE /api/appointments/:id
- **Description:** Deletes an appointment.
- **URL Parameter:** `id`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Sample Response:**
  ```json
  { "message": "Appointment deleted successfully" }
  ```

---

## 13. Seller Upgrade

### POST /api/users/upgrade-to-seller
- **Description:** Initiates the seller upgrade process by creating a payment request. (Only authenticated users can initiate.)
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** User initiates upgrade to seller.
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
- **Description:** Verifies the upgrade payment and updates the user's role to "seller".
- **Request Body:** (JSON)
  ```json
  { "authority": "FAKE_AUTH_CODE_ABC123", "status": "OK" }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** After successful payment, user's role is updated.
- **Sample Response:**
  ```json
  { "message": "User upgraded to seller successfully" }
  ```

---

## 14. Social Bookmark with Notes

### POST /api/share/bookmark
- **Description:** Saves a property to the user's wishlist with an optional personal note and generates a shareable URL.
- **Request Body:** (JSON)
  ```json
  {
    "houseId": 2,
    "note": "این ملک با موقعیت عالی مناسب سرمایه‌گذاری است."
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Users bookmark properties with notes and share the URL.
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

## 15. Targeted Notifications

### POST /api/notifications/settings
- **Description:** Creates a new targeted notification setting.
- **Request Body:** (JSON)
  ```json
  {
    "notificationType": "price_drop",
    "criteria": { "priceThreshold": 1000000 }
  }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Users set notification preferences.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Display current notification settings.
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
- **URL Parameter:** `id`
- **Request Body:** (JSON)
  ```json
  { "criteria": { "priceThreshold": 900000 } }
  ```
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Modify a notification setting.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Remove a notification setting.
- **Sample Response:**
  ```json
  { "message": "Notification setting deleted successfully" }
  ```

### POST /api/notifications/send-scheduled
- **Description:** Simulates sending scheduled notifications.
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Typically triggered by a background job.
- **Sample Response:**
  ```json
  { "message": "Scheduled notifications sent", "settings": [ /* list of settings */ ] }
  ```

---

## 16. Recommendation System

### GET /api/recommendations/:userId
- **Description:** Retrieves property recommendations for a user based on historical data.
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Provides intelligent property suggestions.
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
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Provides an estimated price for evaluation.
- **Sample Response:**
  ```json
  { "predictedPrice": 190000 }
  ```

---

## 17. User Activity Dashboard

### GET /api/users/activity/:userId
- **Description:** Retrieves activity data for a user, including:
  - **Number of Bookings:** Total bookings made by the user.
  - **Feedback Given:** Count of feedback entries the user has submitted.
  - **Feedback Received:** Count of feedback entries received by the user.
  - **Properties Created:** Total properties created by the user (if the user is a seller).
- **URL Parameter:** `userId`
- **Headers:**  
  - `Authorization: Bearer <token>`
- **Usage Scenario:** Used by users and admins to review user engagement and activity.
- **Sample Request:**  
  `GET /api/users/activity/1`
- **Sample Response:**
  ```json
  {
    "userId": 1,
    "bookingCount": 5,
    "feedbackGiven": 3,
    "feedbackReceived": 4,
    "housesCreated": 2
  }
  ```

---


## Scenarios Overview – Detailed Workflow and Use Cases

This section provides a comprehensive overview of various user scenarios that our Real Estate API supports. It outlines step-by-step workflows, the interactions between endpoints, and the expected behaviors for different use cases. Each scenario is designed to cover the full spectrum of functionality – from core actions (like property browsing, booking, and payment) to additional features (like feedback, notifications, and seller upgrades).

---

### 1. User Registration & Login

**Scenario: New User Registration and Authentication**

- **Workflow:**
  1. **Registration:**  
     - **Endpoint:** `POST /api/users/register`  
     - **Input:**  
       - Required fields: `email`, `password`, `fullName`, `firstName`, `lastName`  
       - Role is hard-coded to `"buyer"`.
     - **Process:**  
       - The system validates the input. If any field is missing, it returns a 400 error.
       - Password is hashed using bcrypt.
       - A new user is created in the database with default values for optional fields (like profile picture).
     - **Output:**  
       - A JSON object containing the user details (with an auto-generated `id` and default `profile_picture`).
  
  2. **Login:**  
     - **Endpoint:** `POST /api/users/login`  
     - **Input:**  
       - `email` and `password`.
     - **Process:**  
       - The system verifies the credentials.
       - If credentials are valid, a JWT access token and a refresh token are issued.
     - **Output:**  
       - Tokens in JSON format.
     - **Usage:**  
       - These tokens are used in the `Authorization` header (`Bearer <token>`) for all protected endpoints.

---

### 2. Property Browsing & Search

**Scenario: User Browses Properties**

- **Workflow:**
  1. **Listing Properties:**  
     - **Endpoint:** `GET /api/houses`  
     - **Input:**  
       - Optional query parameters for pagination (`page`, `limit`), sorting (`sort`, `order`), and filtering (e.g., `transaction_type`, `price`, etc.).
     - **Process:**  
       - The system fetches property records from the database based on the given filters.
     - **Output:**  
       - A paginated list of properties in JSON format.
  
  2. **Viewing Property Details:**  
     - **Endpoint:** `GET /api/houses/:id`  
     - **Input:**  
       - Property `id` as URL parameter.
     - **Process:**  
       - Retrieves full details of the selected property (including photos, caption, and seller information).
     - **Output:**  
       - Detailed property information in JSON format.
  
  3. **Geo-Search:**  
     - **Endpoint:** `GET /api/houses/geo-search`  
     - **Input:**  
       - `lat`, `lng`, `radius` as query parameters.
     - **Process:**  
       - The system uses geographic functions to return properties within the specified radius.
     - **Output:**  
       - A list of nearby properties.

---

### 3. Booking & Payment Processing

**Scenario: User Books a Property and Completes Payment**

- **Workflow:**
  1. **Creating a Booking:**  
     - **Endpoint:** `POST /api/bookings`  
     - **Input:**  
       - `houseId`, `reservedDates` (as a date range), and `traveler_details` (an array of passenger objects with fields like `firstName`, `lastName`, `gender`, `birthDate`, `nationalId`).
       - Optional: `sharedEmail` and `sharedMobile` for ticket sharing.
     - **Process:**  
       - The booking is created with a default status `"pending"`.
     - **Output:**  
       - The new booking object is returned in JSON.
  
  2. **Initiating Payment:**  
     - **Endpoint:** `POST /api/payments/checkout`  
     - **Input:**  
       - `bookingId` and `amount`.
     - **Process:**  
       - The system generates a payment request and returns an authority code and payment URL.
     - **Output:**  
       - Payment details in JSON.
  
  3. **Verifying Payment:**  
     - **Endpoint:** `POST /api/payments/verify`  
     - **Input:**  
       - `authority`, `status` (e.g., "OK"), and `bookingId`.
     - **Process:**  
       - If the payment status is confirmed (e.g., "OK"), the system updates the booking status to `"confirmed"`.
       - At this point, if `sharedEmail` or `sharedMobile` is provided in the booking, a ticket is sent via email and SMS (using the TicketNotificationService).
     - **Output:**  
       - Confirmation message and updated booking status.
  
  4. **Canceling or Continuing a Booking:**  
     - **Cancel:**  
       - **Endpoint:** `POST /api/bookings/:id/cancel`
       - **Process:**  
         - Changes booking status from `"pending"` to `"canceled"`.
       - **Output:**  
         - Cancellation confirmation.
     - **Continue:**  
       - **Endpoint:** `POST /api/bookings/:id/continue`
       - **Process:**  
         - Changes status from `"canceled"` back to `"pending"`.
       - **Output:**  
         - Updated booking status confirmation.

---

### 4. Feedback, Ratings & Q&A

**Scenario: User Provides Feedback and Asks/Answers Questions**

- **Feedback:**  
  - **Endpoint:** `POST /api/feedback`  
  - **Input:**  
    - `reviewerId`, `revieweeId`, `rating`, `comment`, and `pointsAwarded`.
  - **Process:**  
    - A feedback entry is created, contributing to the loyalty system.
  - **Output:**  
    - Feedback details.
  
- **Q&A:**  
  - **Ask Question:**  
    - **Endpoint:** `POST /api/property-qas/question`  
    - **Input:**  
      - `houseId` and `question`.
    - **Process:**  
      - The question is recorded.
    - **Output:**  
      - Question details.
  - **Answer Question:**  
    - **Endpoint:** `POST /api/property-qas/answer`  
    - **Input:**  
      - `questionId` and `answer`.
    - **Process:**  
      - The answer is saved and linked to the question.
    - **Output:**  
      - Updated Q&A details.
  - **Retrieve Q&A:**  
    - **Endpoint:** `GET /api/property-qas/:houseId`  
    - **Process:**  
      - All questions and answers for the property are returned.
    - **Output:**  
      - List of Q&A entries.

---

### 5. Price History Tracking

**Scenario: User Analyzes Historical Price Data for a Property**

- **Retrieve Full Price History:**  
  - **Endpoint:** `GET /api/price-history/:houseId`
  - **Process:**  
    - The system returns a chronological list of recorded price changes.
  - **Output:**  
    - Price history records with timestamps.
  
- **Monthly Percentage Changes:**  
  - **Endpoint:** `GET /api/price-history/:houseId/monthly-changes`
  - **Process:**  
    - The system calculates and returns the percentage change in price for each month.
  - **Output:**  
    - A list of monthly changes with percentage values.

---

### 6. Social Authentication & Sharing

**Scenario: User Logs in via Social Platforms and Shares Content**

- **Social Authentication:**  
  - **Endpoints:**  
    - `/api/auth/social/facebook` and `/api/auth/social/facebook/callback`
    - `/api/auth/social/google` and `/api/auth/social/google/callback`
    - `/api/auth/social/linkedin` and `/api/auth/social/linkedin/callback`
  - **Process:**  
    - User is redirected to the social platform for authentication.
    - Upon successful authentication, a JWT token is issued.
  
- **Social Sharing:**  
  - **Endpoints:**  
    - `GET /api/share/property` – Generates a shareable URL for a property.
    - `GET /api/share/comment` – Generates a shareable URL for a comment.
  - **Usage Scenario:**  
    - Users share property or comment details on social media.
  - **Output:**  
    - A URL for sharing.

---

### 7. Virtual Open House Scheduling

**Scenario: User Schedules a Property Visit**

- **Creating an Appointment:**  
  - **Endpoint:** `POST /api/appointments`
  - **Input:**  
    - `houseId`, `appointmentTime`, and `type` (either "virtual" or "in_person").
  - **Process:**  
    - An appointment is created with status `"pending"`.
  - **Output:**  
    - Appointment details.
  
- **Retrieving Appointments:**  
  - **For a Property:** `GET /api/appointments/house/:houseId`
  - **For the Authenticated User:** `GET /api/appointments/user`
  - **Usage Scenario:**  
    - Users view scheduled appointments to plan their visits.
  
- **Updating/Deleting Appointments:**  
  - **Endpoint for Update:** `PUT /api/appointments/:id`
  - **Endpoint for Delete:** `DELETE /api/appointments/:id`
  - **Process:**  
    - Users can modify or cancel appointments as needed.
  - **Output:**  
    - Confirmation messages and updated appointment details.

---

### 8. Seller Upgrade

**Scenario: User Upgrades to Seller**

- **Initiating Upgrade:**  
  - **Endpoint:** `POST /api/users/upgrade-to-seller`
  - **Process:**  
    - The system creates a payment request for the seller upgrade fee.
  - **Output:**  
    - Payment details including an authority code and a payment URL.
  
- **Verifying Upgrade Payment:**  
  - **Endpoint:** `POST /api/users/upgrade-to-seller/verify`
  - **Input:**  
    - `authority` and `status` (e.g., "OK").
  - **Process:**  
    - Upon successful verification, the user's role is updated to `"seller"`.
  - **Output:**  
    - Confirmation message indicating the upgrade success.

---

### 9. Social Bookmark with Notes

**Scenario: User Bookmarks a Property for Sharing**

- **Creating a Bookmark:**  
  - **Endpoint:** `POST /api/share/bookmark`
  - **Input:**  
    - `houseId` and `note` (a personal note about the property).
  - **Process:**  
    - The property is saved to the user's wishlist with the note.
    - A shareable URL is generated that includes the bookmark note as a query parameter.
  - **Output:**  
    - Bookmark details and the shareable URL.

---

### 10. Targeted Notifications

**Scenario: User Sets Up Targeted Notification Preferences**

- **Creating Notification Settings:**  
  - **Endpoint:** `POST /api/notifications/settings`
  - **Input:**  
    - `notificationType` (e.g., "price_drop") and `criteria` (e.g., `{ "priceThreshold": 1000000 }`).
  - **Process:**  
    - The setting is saved, linking the criteria with the user.
  - **Output:**  
    - Notification setting details.
  
- **Retrieving, Updating, and Deleting Settings:**  
  - **Endpoints:**  
    - `GET /api/notifications/settings`
    - `PUT /api/notifications/settings/:id`
    - `DELETE /api/notifications/settings/:id`
  - **Usage Scenario:**  
    - Users can view and manage their notification preferences.
  
- **Sending Scheduled Notifications:**  
  - **Endpoint:** `POST /api/notifications/send-scheduled`
  - **Process:**  
    - A background job (or manual trigger) sends notifications based on the saved settings.
  - **Output:**  
    - Confirmation message with a list of settings processed.

---

### 11. User Activity Dashboard

**Scenario: User or Admin Reviews Activity Data**

- **Endpoint:** `GET /api/users/activity/:userId`
- **Process:**  
  - Retrieves aggregated activity data for the specified user:
    - **Booking Count:** Total number of bookings made.
    - **Feedback Given:** Count of feedback entries submitted.
    - **Feedback Received:** Count of feedback entries received.
    - **Properties Created:** Total properties listed by the user (if seller).
- **Usage Scenario:**  
  - Users can view their activity summary; admins can monitor user engagement.
- **Output:**  
  - A JSON object containing the aggregated counts.

---

## Summary

These detailed scenarios outline the complete user journey across all aspects of the Real Estate API. From user registration, property browsing, booking with payment processing, through to additional features like social sharing, notifications, and activity tracking, the API supports a rich set of interactions. Each scenario includes:

- **Endpoints Involved:** Clear mapping of endpoints used at each step.
- **Input & Output Requirements:** Detailed information on required request parameters, headers, and sample responses.
- **Step-by-Step Workflow:** A comprehensive description of the process flow, including validation, processing, and post-processing steps.
- **Usage Scenarios:** Real-world examples of how a user or admin would interact with the API to accomplish tasks.

This comprehensive overview ensures that frontend developers, as well as other stakeholders, understand the full capabilities of the API and can integrate it effectively into the application.

---

This documentation covers all approximately 78 endpoints in the Real Estate API, detailing request parameters, headers, sample requests/responses, and usage scenarios for each category. It serves as a complete reference for frontend developers to integrate and work with the API.
