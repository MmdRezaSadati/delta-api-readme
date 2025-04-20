## Level 1 (Most Important APIs)

1. **Houses API**
   - `GET    /api/houses`
   - `GET    /api/houses/:id`
   - `POST   /api/houses`
   - `PUT    /api/houses/:id`
   - `DELETE /api/houses/:id`
   - **Management**: `GET /api/admin/houses`

2. **Users API**
   - `POST   /api/users/register`
   - `POST   /api/users/login`
   - `GET    /api/users`
   - `GET    /api/users/:id`
   - `PUT    /api/users/:id`
   - `DELETE /api/users/:id`
   - **Management**: `GET /api/admin/users`

3. **Bookings API**
   - `GET    /api/bookings`
   - `GET    /api/bookings/:id`
   - `POST   /api/bookings`
   - `PUT    /api/bookings/:id`
   - `DELETE /api/bookings/:id`

4. **Categories API**
   - `GET    /api/categories`
   - `GET    /api/categories/:id`
   - `POST   /api/categories`
   - `PUT    /api/categories/:id`
   - `DELETE /api/categories/:id`

5. **Accommodations API**
   - `GET    /api/accommodations`
   - `GET    /api/accommodations/:id`
   - `POST   /api/accommodations`
   - `PUT    /api/accommodations/:id`
   - `DELETE /api/accommodations/:id`

6. **Contact Us API**
   - `GET    /api/contact-us`
   - `GET    /api/contact-us/:id`
   - `POST   /api/contact-us`
   - `DELETE /api/contact-us/:id`

7. **Discount Codes API**
   - `GET    /api/discount-codes`
   - `GET    /api/discount-codes/:id`
   - `POST   /api/discount-codes`
   - `PUT    /api/discount-codes/:id`
   - `DELETE /api/discount-codes/:id`

8. **Favorites API**
   - `POST   /api/favorites/add`
   - `DELETE /api/favorites/remove`
   - `GET    /api/favorites/:user_id`

9. **Locations API**
   - `GET    /api/locations`
   - `GET    /api/locations/:id`
   - `POST   /api/locations`
   - `PUT    /api/locations/:id`
   - `DELETE /api/locations/:id`

10. **Payment API**
    - `POST /api/payments/create`
    - `POST /api/payments/verify`

11. **Seller Upgrade API**
    - `POST /api/seller/upgrade-to-seller`
    - `POST /api/seller/upgrade-to-seller/verify`

12. **Comments API**
    - `GET    /api/comments`
    - `GET    /api/comments/:id`
    - `POST   /api/comments`
    - `PUT    /api/comments/:id`
    - `DELETE /api/comments/:id`
    - **Management**: `GET /api/admin/comments`



---



## Level 2 (Secondary APIs)

1. **Blogs API**
   - `GET    /api/blogs`
   - `GET    /api/blogs/:id`
   - `POST   /api/blogs`
   - `PUT    /api/blogs/:id`
   - `DELETE /api/blogs/:id`

2. **Social Media Links API**
   - `GET    /api/social-media-links`
   - `GET    /api/social-media-links/:id`
   - `POST   /api/social-media-links`
   - `PUT    /api/social-media-links/:id`
   - `DELETE /api/social-media-links/:id`

3. **Price History API**
   - `GET /api/price-history/:houseId`
   - `GET /api/price-history/:houseId/monthly-changes`

4. **Property QA API**
   - `POST /api/property-QA/question`
   - `POST /api/property-QA/answer`
   - `GET  /api/property-QA/:houseId`

5. **Documents API**
   - `GET    /api/documents/:id`
   - `POST   /api/documents`
   - `DELETE /api/documents/:id`

6. **User Activity API**
   - `GET /api/user-activity/:userId`

7. **Notification API**
   - `POST /api/notifications/send`

8. **Targeted Notification API**
   - `POST   /api/targeted-notification/settings`
   - `GET    /api/targeted-notification/settings`
   - `DELETE /api/targeted-notification/settings/:id`

9. **Ticket Notification API**
   - `POST /api/ticket-notification/send`

10. **Feedback API**
    - `GET /api/feedback/received/:userId`
    - `GET /api/feedback/given/:userId`
    - `GET /api/feedback/loyalty/:userId`

11. **Recommendation API**
    - `GET  /api/recommendations/:userId`
    - `POST /api/recommendations/predict`

12. **Public Profile API**
    - `GET /api/public-profiles/:id`

13. **Comparison API**
    - `GET /api/compare-properties`

14. **Saved Search API**
    - `GET    /api/saved-search`
    - `POST   /api/saved-search`
    - `DELETE /api/saved-search/:id`

15. **Social Bookmark API**
    - `GET    /api/social-bookmark`
    - `POST   /api/social-bookmark`
    - `DELETE /api/social-bookmark/:id`
