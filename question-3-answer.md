Q3: Functional & Non-Functional Requirements

Functional Requirements
1. FR1: Truck Location Tracking
   System tracks truck GPS coordinates in real-time and displays live map views.

2. FR2: Driver Management  
   Register, view, update and delete driver profiles with license and contact information.

3. FR3: Schedule Assignment  
   Admins can assign delivery jobs to available drivers with time slots and destinations.

4. FR4: Container Assignment  
   Link containers to trucks, including load status (e.g., loaded, in transit, delivered).

5. FR5: Route Planning  
   System suggests optimal delivery routes based on distance and estimated fuel usage.

6. FR6: Delivery Tracking  
   Update delivery status (e.g., en route, delayed, completed) and timestamp logs.

7. FR7: Customer Management  
   Manage customer profiles and order history for invoicing and service tracking.

8. FR8: Order & Billing Generation  
   Create delivery orders, calculate costs, and generate PDF invoices for clients.

9. FR9: Maintenance & Fuel Log  
   Record truck maintenance schedules and fuel costs with date, and cost.

10. FR10: Notification System 
    Send alerts to drivers and clients about upcoming schedules, updates, or delays.

Non-Functional Requirements
- NFR1: Performance  
  The system should respond to user inputs within 2 seconds.

- NFR2: Availability  
  The system should ensure 99.5% uptime for 24/7 logistics operation.

- NFR3: Reliability  
  The system should maintain consistent operation with a failure rate of 0.2% during daily logistics operations.

- NFR4: Cross-platform Compatibility  
  System must work smoothly on both desktop and mobile browsers.

Technology Justification
- Backend: Node.js + Express — scalable, event-driven, large ecosystem for real‑time features.  (PYTHON3)
- Database: MySQL — relational model fits transactional integrity for truck, driver & billing data.  
- Frontend: React + Tailwind CSS — component‑driven, responsive UI, quick iteration.  
- Hosting: AWS Elastic Beanstalk with RDS — managed scalability, high availability SLA.

