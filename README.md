# Real Estate API Project Documentation

## Introduction

This project provides a comprehensive API for managing real estate properties, along with additional features such as house reservations, user comments, blog posts, user management, and more. The API is implemented using **Node.js**, **Express.js**, and **TypeScript**. Additionally, Sequelize is used as the ORM for database interactions.

# Technical Details

## List of Endpoints and Their Descriptions

### 1. **Properties (Houses)**

- **GET /api/houses**  
  Retrieve all houses with support for filtering (e.g., title, address, rate, price, capacity, transaction_type), pagination (using `page` and `limit` parameters), and sorting (using `sort` and `order` parameters).

- **GET /api/houses/:id**  
  Retrieve details of a specific house by its ID.

- **POST /api/houses**  
  Create a new house. The input data must include the required fields defined in the House model.

- **PUT /api/houses/:id**  
  Update a house's information based on its ID.

- **DELETE /api/houses/:id**  
  Delete a house by its ID.

---

### 2. **Users**

- **POST /api/users/register**  
  Register a new user. The password is hashed during this process.

- **POST /api/users/login**  
  User login. If the password matches, a JWT token is issued.

- **GET /api/users**  
  Retrieve all users with filtering support based on role or registration date.

- **GET /api/users/:id**  
  Retrieve details of a specific user by their ID.

- **PUT /api/users/:id**  
  Update a user's information. If the password is changed, it is re-hashed.

- **DELETE /api/users/:id**  
  Delete a user by their ID.

---

### 3. **Bookings**

- **GET /api/bookings**  
  Retrieve all bookings with support for filtering (e.g., house_id), pagination, and sorting.

- **GET /api/bookings/:id**  
  Retrieve details of a specific booking by its ID.

- **POST /api/bookings**  
  Create a new booking.

- **PUT /api/bookings/:id**  
  Update a booking's information.

- **DELETE /api/bookings/:id**  
  Delete a booking by its ID.

---

### 4. **Comments & Ratings**

- **GET /api/comments**  
  Retrieve all comments with filtering support based on house_id, user_id, or rating.

- **GET /api/comments/:id**  
  Retrieve details of a specific comment by its ID.

- **POST /api/comments**  
  Create a new comment and rating.

- **PUT /api/comments/:id**  
  Update a comment or rating.

- **DELETE /api/comments/:id**  
  Delete a comment by its ID.

---

### 5. **Payment & Reservations**

- **POST /api/payments/checkout**  
  Initiate a payment for a booking.

- **GET /api/payments/status/:transactionId**  
  Check the payment status of a transaction.

- **POST /api/payments/verify**  
  Verify a payment after a successful transaction.

---

### 6. **Dashboard & Analytics**

- **GET /api/dashboard/summary**  
  Retrieve an overview of houses, users, bookings, and average rating.

- **GET /api/dashboard/reports**  
  Retrieve various analytical reports on user activity and bookings.

---

### 7. **Blogs**

- **GET /api/blogs**  
  Retrieve all blog posts with filtering support based on title, author_id, or category_id.

- **GET /api/blogs/:id**  
  Retrieve details of a specific blog post by its ID.

- **POST /api/blogs**  
  Create a new blog post.

- **PUT /api/blogs/:id**  
  Update a blog post.

- **DELETE /api/blogs/:id**  
  Delete a blog post by its ID.

---

### 8. **Categories**

- **GET /api/categories**  
  Retrieve all categories.

- **GET /api/categories/:id**  
  Retrieve details of a specific category by its ID.

- **POST /api/categories**  
  Create a new category.

- **PUT /api/categories/:id**  
  Update a category.

- **DELETE /api/categories/:id**  
  Delete a category by its ID.

---

### 10. **Favorites**

- **POST /api/favorites/add**  
  Add a house to a user's favorites. The request body must include `user_id` and `house_id`.

- **DELETE /api/favorites/remove**  
  Remove a house from a user's favorites. The request body must include `user_id` and `house_id`.

- **GET /api/favorites/:user_id**  
  Retrieve all favorite houses for a specific user by their `user_id`, with pagination support.

---

### 11. **Locations**

- **GET /api/locations**  
  Retrieve all locations (e.g., areas) with filtering support based on area_name, lat, or lng.

- **GET /api/locations/:id**  
  Retrieve details of a specific location by its ID.

- **POST /api/locations**  
  Create a new location.

- **PUT /api/locations/:id**  
  Update a location.

- **DELETE /api/locations/:id**  
  Delete a location by its ID.

---

### 12. **Social Media Links**

- **GET /api/social-media-links**  
  Retrieve all social media links with filtering support based on platform or URL.

- **GET /api/social-media-links/:id**  
  Retrieve details of a specific social media link by its ID.

- **POST /api/social-media-links**  
  Create a new social media link.

- **PUT /api/social-media-links/:id**  
  Update a social media link.

- **DELETE /api/social-media-links/:id**  
  Delete a social media link by its ID.

---

