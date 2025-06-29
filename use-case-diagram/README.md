Airbnb Clone Use Case Diagram

Objective

This document presents a use case diagram for the Airbnb Clone backend, visualizing the interactions between actors (Guest, Host, Admin, and System) and the system for key functionalities, including user registration, property booking, payments, and more. The diagram is created using Mermaid syntax and can be imported into Draw.io for visualization and export as a PNG.

Actors

Guest: A user who searches for and books properties, makes payments, and leaves reviews.

Host: A user who creates, manages, and responds to reviews for property listings.

Admin: A user who manages users, listings, bookings, and payments via an admin dashboard.

System: The backend system handling authentication, notifications, and third-party integrations.

Use Cases

The following use cases are derived from the core functionalities:

User Registration: Users sign up as Guest or Host, including OAuth options.

User Login: Users log in via email/password or OAuth.

Profile Management: Users update their profile details.

Create/Update/Delete Property Listing: Hosts manage their property listings.

Search Properties: Guests search and filter properties by location, price, etc.

![alt text](https://github.com/Sherif425/alx-airbnb-project-documentation/blob/main/use-case-diagram/use_case.drawio.png)


Book Property: Guests create bookings for available dates.


Cancel Booking: Guests or Hosts cancel bookings per policy.


Make Payment: Guests process payments for bookings.


Leave Review: Guests rate and comment on properties post-stay.


Respond to Review: Hosts reply to guest reviews.


Receive Notifications: Users get email/in-app notifications for bookings, payments, etc.




Manage System (Admin): Admins monitor and manage users, listings, bookings, and payments.


