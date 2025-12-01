As an expert Agile Product Owner, I've crafted a comprehensive set of User Stories for a "такси ordering service," adhering strictly to the INVEST principles (Independent, Negotiable, Valuable, Estimable, Small, Testable). Key stories include detailed Acceptance Criteria to ensure clarity and testability.

---

### **User Stories for Taxi Ordering Service**

Here's a breakdown of user stories, categorized by user role and core functionality:

#### **Passenger (Rider) Stories**

*   **Registration & Profile**
    *   As a passenger, I want to register for an account using my phone number, so that I can access the taxi service.
    *   As a passenger, I want to update my personal profile information (name, email), so that my account details are accurate.
    *   As a passenger, I want to view my ride history, so that I can keep track of my past trips and expenses.

*   **Ordering a Ride**
    *   As a passenger, I want to easily request a ride to my chosen destination, so that I can quickly get transportation.
        *   **Acceptance Criteria:**
            *   User can input destination via text search or by dropping a pin on the map.
            *   System automatically detects and displays the user's current location as the pickup point.
            *   User can optionally adjust the pickup location within a reasonable radius.
            *   System displays a list of available car types (e.g., Economy, Comfort, Van) with estimated fare ranges and arrival times for each.
            *   User can select a preferred car type.
            *   User can confirm the ride request with a single action, initiating the driver search.
            *   A confirmation message is displayed after the request is sent.
    *   As a passenger, I want to select a specific car category (e.g., Economy, Business, Van), so that I can choose a vehicle that meets my needs and budget.
    *   As a passenger, I want to see the estimated fare and arrival time before confirming my ride, so that I can make an informed decision.
    *   As a passenger, I want to add multiple stops to my ride, so that I can pick up or drop off others along the way.
    *   As a passenger, I want to schedule a ride for a future time and date, so that I can plan my transportation in advance.

*   **During the Ride**
    *   As a passenger, I want to see my assigned driver's real-time location and details, so that I know who to expect and when they will arrive.
        *   **Acceptance Criteria:**
            *   After a driver accepts, the app displays the driver's current position on a map, updating in real-time.
            *   The driver's name, photo, vehicle make/model, license plate number, and rating are prominently displayed.
            *   An estimated time of arrival (ETA) for the driver to the pickup location is shown.
            *   The passenger can initiate a call or chat with the driver directly from this screen.
    *   As a passenger, I want to cancel my ride request before pickup, so that I have flexibility if my plans change.
    *   As a passenger, I want to communicate with my driver via in-app chat or call, so that I can provide instructions or ask questions.

*   **Payment & Feedback**
    *   As a passenger, I want to securely add and manage multiple payment methods (credit/debit card, mobile pay), so that I have flexibility in how I pay for my rides.
        *   **Acceptance Criteria:**
            *   User can navigate to a "Payment Methods" section in their profile.
            *   User can add a new credit/debit card by securely entering card number, expiry date, and CVV.
            *   System tokenizes and stores card details, displaying only the last four digits.
            *   User can set a default payment method.
            *   User can delete existing payment methods (excluding the default if it's the only one).
            *   Payment method selection is available on the ride confirmation screen before requesting a ride.
    *   As a passenger, I want to pay for my ride automatically using my chosen payment method upon completion, so that I don't need to handle cash.
    *   As a passenger, I want to rate my driver and the overall ride experience, so that I can provide feedback on the service.
    *   As a passenger, I want to receive a digital receipt for my ride via email or in-app, so that I have a record of my payment.
    *   As a passenger, I want to apply promotional codes or discounts to my ride, so that I can save money.

#### **Driver Stories**

*   **Registration & Availability**
    *   As a driver, I want to register for an account by submitting my personal and vehicle documents, so that I can become an approved taxi driver.
    *   As a driver, I want to easily toggle my online/offline status, so that I can control when I receive ride requests.
        *   **Acceptance Criteria:**
            *   A prominent toggle switch or button is available on the driver's main screen to go "Online" or "Offline".
            *   When "Online", the driver's location is visible to the system for dispatching requests.
            *   When "Offline", the driver does not receive ride requests.
            *   The app clearly indicates the current online/offline status to the driver.
    *   As a driver, I want to update my vehicle information and personal documents, so that my profile remains current.

*   **Ride Management**
    *   As a driver, I want to receive clear ride requests with essential details and have the option to accept or decline, so that I can make informed decisions.
        *   **Acceptance Criteria:**
            *   When a new ride request is assigned, the driver receives an audible and visual notification.
            *   The request displays the passenger's pickup location, destination, estimated distance, estimated fare, and passenger rating.
            *   The driver has a limited time window (e.g., 15-20 seconds) to tap "Accept" or "Decline".
            *   If accepted, the system confirms the ride and navigates the driver to the pickup location.
            *   If declined or timed out, the request is routed to another driver.
    *   As a driver, I want in-app navigation to the passenger's pickup location and then to the destination, so that I can efficiently complete the ride.
    *   As a driver, I want to mark the ride as "Arrived at Pickup," "Passenger Picked Up," and "Ride Completed," so that the ride status is accurately updated and fare calculation is correct.
    *   As a driver, I want to communicate with my passenger via in-app chat or call, so that I can coordinate pickup or clarify details.
    *   As a driver, I want to cancel a ride due to valid reasons (e.g., passenger no-show), so that I am not penalized.

*   **Earnings & Feedback**
    *   As a driver, I want to view my earnings summary (daily, weekly, monthly), so that I can track my income.
    *   As a driver, I want to rate my passenger, so that I can provide feedback on their conduct.
    *   As a driver, I want to see details of my past rides, so that I can review my work history.

#### **Administrator/Operator Stories**

*   **User & Driver Management**
    *   As an administrator, I want to review and approve new driver registrations, so that I can ensure all drivers meet regulatory and service quality standards.
        *   **Acceptance Criteria:**
            *   New driver registrations appear in a dedicated "Pending Approval" queue in the admin panel.
            *   For each pending driver, the admin can view submitted documents (e.g., driver's license, vehicle registration, insurance).
            *   The admin can approve the driver, making them eligible to go online.
            *   The admin can reject the driver, optionally providing a reason that is communicated to the driver.
            *   The admin can filter and search pending drivers.
    *   As an administrator, I want to view and manage all passenger accounts, so that I can provide support or resolve issues.
    *   As an administrator, I want to deactivate or block driver/passenger accounts, so that I can enforce platform policies.

*   **Ride & Fare Management**
    *   As an administrator, I want to view real-time and historical ride data, so that I can monitor operations and investigate issues.
    *   As an administrator, I want to configure pricing rules based on car type, time of day, and demand (surge pricing), so that I can optimize revenue and market competitiveness.
        *   **Acceptance Criteria:**
            *   Admin panel has a "Pricing" configuration section.
            *   Admin can define base fare, per-kilometer rate, and per-minute rate for each car category.
            *   Admin can create time-based surcharges (e.g., night rates) or location-based surcharges.
            *   Admin can set surge multipliers that apply automatically during high-demand periods, with configurable triggers.
            *   All changes can be previewed before going live and are logged for auditing.
    *   As an administrator, I want to manage promotional codes and discounts, so that I can run marketing campaigns.

*   **Reporting & Support**
    *   As an administrator, I want to generate reports on key metrics (e.g., number of rides, earnings, driver activity), so that I can analyze business performance.
    *   As an administrator, I want to handle customer support tickets and dispute resolution, so that I can ensure a fair and efficient service.

---