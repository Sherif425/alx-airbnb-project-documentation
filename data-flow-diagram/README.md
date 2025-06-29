Airbnb Clone Data Flow Diagram
Objective
This document presents a Level-1 Data Flow Diagram (DFD) for the Airbnb Clone backend, illustrating how data moves through the system for core functionalities, including user management, property listings, booking, payments, reviews, notifications, and admin operations. The DFD is created using Mermaid syntax and can be imported into Draw.io for visualization and export as a PNG.
DFD Overview
The DFD maps data flows between external entities (Guest, Host, Admin, Payment Gateway, Notification Service), processes, and data stores (Users, Properties, Bookings, Payments, Reviews, Messages). It covers key inputs, processes, and outputs for the backend operations.
External Entities

Guest: Interacts with the system to register, log in, search properties, book properties, make payments, and leave reviews.
Host: Interacts to register, log in, manage properties, respond to reviews, and cancel bookings.
Admin: Manages users, listings, bookings, and payments.
Payment Gateway: Processes payments (e.g., Stripe, PayPal).
Notification Service: Sends email/in-app notifications (e.g., SendGrid).

Data Stores

Users: Stores user details (user_id, email, role, etc.).
Properties: Stores property details (property_id, name, price, etc.).
Bookings: Stores booking details (booking_id, user_id, property_id, dates, status).
Payments: Stores payment details (payment_id, booking_id, amount, method).
Reviews: Stores review details (review_id, property_id, rating, comment).
Messages: Stores communication (message_id, sender_id, recipient_id, body).

Processes

Register/Login User: Handles user registration and authentication.
Manage Profile: Updates user profile details.
Manage Properties: Creates, updates, or deletes property listings.
Search Properties: Searches and filters properties based on criteria.
Process Booking: Creates and validates bookings.
Handle Payment: Processes payments via external gateways.
Manage Reviews: Handles guest reviews and host responses.
Send Notifications: Sends email/in-app notifications.
Admin Management: Manages users, listings, bookings, and payments.

Data Flow Diagram
