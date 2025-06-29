Airbnb Clone Property Booking Flowchart
Objective
This document presents a flowchart for the property booking process in the Airbnb Clone backend, visualizing the steps and data flow from a guest submitting a booking request to the final confirmation or rejection. The flowchart is created using Mermaid syntax and can be imported into Draw.io for visualization and export as a PNG.
Process Overview
The property booking process involves a Guest submitting a booking request, the system validating the request, processing payment, creating a booking record, and sending notifications. The flowchart covers:

Actors: Guest, System (backend), Payment Gateway, Notification Service.
Data Stores: Properties, Bookings, Payments.
Steps: Authentication, date validation, cost calculation, payment processing, booking creation, and notification sending.
Decision Points: Guest authentication, property availability, payment success.



Flowchart Explanation

Start: The Guest submits a booking request with property ID, start/end dates, and payment details.
Processes:
Authenticate Guest: Verifies the Guest’s JWT to ensure they are logged in.
Retrieve Property Details: Fetches property data (e.g., price per night) from the Properties data store.
Check Date Availability: Validates that the requested dates do not conflict with existing bookings in the Bookings data store.
Calculate Total Cost: Computes cost using price per night and booking duration.
Process Payment: Sends payment details to the Payment Gateway (e.g., Stripe/PayPal).
Create Booking Record: Stores the booking (status: confirmed) in the Bookings data store.
Store Payment Record: Records payment details in the Payments data store.
Send Notifications: Sends confirmation to Guest and new booking notification to Host via Notification Service.


Decision Points:
Authenticate Guest: Checks if the JWT is valid; rejects if unauthorized.
Check Date Availability: Ensures no overlapping bookings; rejects if dates are unavailable.
Process Payment: Verifies payment success; rejects if payment fails.


Data Stores:
Properties: Provides property details (e.g., price per night).
Bookings: Stores booking records and checks for date conflicts.
Payments: Stores payment records.


External Entity:
Notification Service: Sends email/in-app notifications (e.g., via SendGrid).


Outputs:
Booking Confirmed: Successful booking with notifications sent.
Request Rejected: Rejection due to authentication failure, unavailable dates, or payment failure.


