# FleetFlow Management System - Database Design & Backend Documentation

## Team Member 2: Database Designer & Backend Lead
**Name:** ANDI KHAIRUL ANUAR BIN AHMAD
**Student ID:** TG21018

---

## 1. DATABASE DESIGN & DATA MODELING

### 1.1 Database Schema Overview
The FleetFlow Management System requires a relational database structure to handle logistics operations efficiently. The database consists of 6 main tables with proper foreign key relationships.

### 1.2 Entity Relationship Diagram
```
DRIVERS (1) -----> (M) SCHEDULES (M) <----- (1) CUSTOMERS
   |                      |                     |
   |                      |                     |
   v                      v                     v
TRUCKS (1) -----> (M) CONTAINERS           ORDERS (1) -----> (M) BILLING
```

### 1.3 Table Structures

#### Table 1: DRIVERS
| Field Name | Data Type | Constraints | Description |
|------------|-----------|-------------|-------------|
| driver_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique driver identifier |
| full_name | VARCHAR(100) | NOT NULL | Driver's full name |
| license_number | VARCHAR(20) | UNIQUE, NOT NULL | Driver's license number |
| phone_number | VARCHAR(15) | NOT NULL | Contact number |
| email | VARCHAR(100) | UNIQUE | Email address |
| hire_date | DATE | NOT NULL | Employment start date |
| status | ENUM('active', 'inactive', 'on_leave') | DEFAULT 'active' | Driver availability status |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation time |

**Sample Data:**
```sql
INSERT INTO DRIVERS VALUES 
(1, 'Ahmad Hafiz bin Rahman', 'D123456789', '0123456789', 'ahmad.hafiz@email.com', '2023-01-15', 'active', '2023-01-15 08:00:00'),
(2, 'Siti Nurhaliza binti Ali', 'D987654321', '0198765432', 'siti.nur@email.com', '2023-02-20', 'active', '2023-02-20 09:30:00'),
(3, 'Kumar Selvam', 'D555666777', '0155566677', 'kumar.s@email.com', '2023-03-10', 'on_leave', '2023-03-10 14:15:00');
```

#### Table 2: TRUCKS
| Field Name | Data Type | Constraints | Description |
|------------|-----------|-------------|-------------|
| truck_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique truck identifier |
| truck_number | VARCHAR(20) | UNIQUE, NOT NULL | Truck registration number |
| model | VARCHAR(50) | NOT NULL | Truck model |
| capacity_tons | DECIMAL(5,2) | NOT NULL | Maximum load capacity |
| fuel_type | ENUM('diesel', 'petrol', 'hybrid') | DEFAULT 'diesel' | Fuel type |
| status | ENUM('available', 'in_use', 'maintenance') | DEFAULT 'available' | Truck status |
| last_maintenance | DATE | | Last maintenance date |
| driver_id | INT | FOREIGN KEY REFERENCES DRIVERS(driver_id) | Assigned driver |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation time |

**Sample Data:**
```sql
INSERT INTO TRUCKS VALUES 
(1, 'KL-1234-A', 'Isuzu NPR', 3.50, 'diesel', 'available', '2024-01-15', 1, '2023-01-20 10:00:00'),
(2, 'SL-5678-B', 'Mitsubishi Canter', 2.80, 'diesel', 'in_use', '2024-02-10', 2, '2023-02-01 11:30:00'),
(3, 'JH-9012-C', 'Hino 300 Series', 4.20, 'diesel', 'maintenance', '2024-01-20', NULL, '2023-03-15 13:45:00');
```

#### Table 3: CUSTOMERS
| Field Name | Data Type | Constraints | Description |
|------------|-----------|-------------|-------------|
| customer_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique customer identifier |
| company_name | VARCHAR(100) | NOT NULL | Customer company name |
| contact_person | VARCHAR(100) | NOT NULL | Primary contact person |
| phone_number | VARCHAR(15) | NOT NULL | Contact number |
| email | VARCHAR(100) | UNIQUE | Email address |
| address | TEXT | NOT NULL | Customer address |
| city | VARCHAR(50) | NOT NULL | City location |
| postal_code | VARCHAR(10) | NOT NULL | Postal code |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation time |

**Sample Data:**
```sql
INSERT INTO CUSTOMERS VALUES 
(1, 'Genting Logistics Sdn Bhd', 'Tan Wei Ming', '0312345678', 'operations@genting.com', 'Jalan Genting, Genting Highlands', 'Genting', '69000', '2023-01-10 09:00:00'),
(2, 'KLCC Properties', 'Sarah Ahmad', '0387654321', 'logistics@klcc.com', 'Kuala Lumpur City Centre', 'Kuala Lumpur', '50088', '2023-01-12 14:20:00'),
(3, 'Port Klang Authority', 'Rajesh Kumar', '0331122334', 'port@klang.gov.my', 'Jalan Pelabuhan Utara, Port Klang', 'Port Klang', '42000', '2023-01-18 16:45:00');
```

#### Table 4: SCHEDULES
| Field Name | Data Type | Constraints | Description |
|------------|-----------|-------------|-------------|
| schedule_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique schedule identifier |
| driver_id | INT | FOREIGN KEY REFERENCES DRIVERS(driver_id) | Assigned driver |
| truck_id | INT | FOREIGN KEY REFERENCES TRUCKS(truck_id) | Assigned truck |
| customer_id | INT | FOREIGN KEY REFERENCES CUSTOMERS(customer_id) | Target customer |
| pickup_location | TEXT | NOT NULL | Pickup address |
| delivery_location | TEXT | NOT NULL | Delivery address |
| scheduled_date | DATE | NOT NULL | Planned delivery date |
| scheduled_time | TIME | NOT NULL | Planned delivery time |
| status | ENUM('pending', 'in_progress', 'completed', 'cancelled') | DEFAULT 'pending' | Schedule status |
| distance_km | DECIMAL(6,2) | | Estimated distance |
| estimated_duration | TIME | | Estimated travel time |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation time |

**Sample Data:**
```sql
INSERT INTO SCHEDULES VALUES 
(1, 1, 1, 1, 'Port Klang Container Terminal', 'Genting Logistics Warehouse', '2024-06-20', '08:00:00', 'pending', 65.30, '02:30:00', '2024-06-19 15:30:00'),
(2, 2, 2, 2, 'Senai Airport Cargo', 'KLCC Loading Bay', '2024-06-21', '09:30:00', 'in_progress', 45.80, '01:45:00', '2024-06-19 16:15:00'),
(3, 1, 1, 3, 'Westport Container Terminal', 'Port Klang Warehouse A', '2024-06-22', '10:00:00', 'pending', 12.50, '00:25:00', '2024-06-19 17:00:00');
```

#### Table 5: CONTAINERS
| Field Name | Data Type | Constraints | Description |
|------------|-----------|-------------|-------------|
| container_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique container identifier |
| container_number | VARCHAR(20) | UNIQUE, NOT NULL | Container reference number |
| truck_id | INT | FOREIGN KEY REFERENCES TRUCKS(truck_id) | Assigned truck |
| schedule_id | INT | FOREIGN KEY REFERENCES SCHEDULES(schedule_id) | Related schedule |
| container_type | ENUM('20ft', '40ft', '45ft') | NOT NULL | Container size |
| weight_kg | DECIMAL(8,2) | NOT NULL | Container weight |
| contents_description | TEXT | | Description of contents |
| load_status | ENUM('empty', 'loaded', 'in_transit', 'delivered') | DEFAULT 'empty' | Container status |
| loaded_at | TIMESTAMP | NULL | Loading timestamp |
| delivered_at | TIMESTAMP | NULL | Delivery timestamp |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation time |

**Sample Data:**
```sql
INSERT INTO CONTAINERS VALUES 
(1, 'CONT-001-2024', 1, 1, '40ft', 15000.00, 'Electronics and Consumer Goods', 'loaded', '2024-06-19 14:30:00', NULL, '2024-06-19 10:15:00'),
(2, 'CONT-002-2024', 2, 2, '20ft', 8500.00, 'Automotive Parts', 'in_transit', '2024-06-19 08:45:00', NULL, '2024-06-19 11:20:00'),
(3, 'CONT-003-2024', 1, 3, '40ft', 12000.00, 'Textile Materials', 'loaded', '2024-06-19 16:00:00', NULL, '2024-06-19 12:30:00');
```

#### Table 6: BILLING
| Field Name | Data Type | Constraints | Description |
|------------|-----------|-------------|-------------|
| billing_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique billing identifier |
| customer_id | INT | FOREIGN KEY REFERENCES CUSTOMERS(customer_id) | Billed customer |
| schedule_id | INT | FOREIGN KEY REFERENCES SCHEDULES(schedule_id) | Related schedule |
| invoice_number | VARCHAR(20) | UNIQUE, NOT NULL | Invoice reference |
| base_rate | DECIMAL(10,2) | NOT NULL | Base delivery rate |
| distance_rate | DECIMAL(10,2) | NOT NULL | Rate per km |
| fuel_surcharge | DECIMAL(10,2) | DEFAULT 0.00 | Additional fuel cost |
| total_amount | DECIMAL(10,2) | NOT NULL | Total bill amount |
| tax_amount | DECIMAL(10,2) | DEFAULT 0.00 | Tax (SST) amount |
| final_amount | DECIMAL(10,2) | NOT NULL | Final payable amount |
| payment_status | ENUM('pending', 'paid', 'overdue') | DEFAULT 'pending' | Payment status |
| invoice_date | DATE | NOT NULL | Invoice generation date |
| due_date | DATE | NOT NULL | Payment due date |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation time |

**Sample Data:**
```sql
INSERT INTO BILLING VALUES 
(1, 1, 1, 'INV-2024-001', 150.00, 2.50, 25.00, 338.25, 20.30, 358.55, 'pending', '2024-06-19', '2024-07-19', '2024-06-19 18:00:00'),
(2, 2, 2, 'INV-2024-002', 120.00, 2.50, 15.00, 249.50, 14.97, 264.47, 'paid', '2024-06-19', '2024-07-19', '2024-06-19 18:30:00'),
(3, 3, 3, 'INV-2024-003', 80.00, 2.50, 10.00, 121.25, 7.28, 128.53, 'pending', '2024-06-19', '2024-07-19', '2024-06-19 19:00:00');
```

---

## 2. API DESIGN

### 2.1 API Endpoint 1: Driver Schedule Assignment

**Endpoint URL:** `POST /api/schedules/assign`

**Description:** Assigns a delivery schedule to an available driver and truck combination.

**Request Headers:**
```
Content-Type: application/json
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "driver_id": 1,
  "truck_id": 1,
  "customer_id": 1,
  "pickup_location": "Port Klang Container Terminal",
  "delivery_location": "Genting Logistics Warehouse",
  "scheduled_date": "2024-06-20",
  "scheduled_time": "08:00:00",
  "distance_km": 65.30,
  "estimated_duration": "02:30:00",
  "container_details": {
    "container_number": "CONT-001-2024",
    "container_type": "40ft",
    "weight_kg": 15000.00,
    "contents_description": "Electronics and Consumer Goods"
  }
}
```

**Response (Success - 201 Created):**
```json
{
  "status": "success",
  "message": "Schedule assigned successfully",
  "data": {
    "schedule_id": 1,
    "driver": {
      "driver_id": 1,
      "full_name": "Ahmad Hafiz bin Rahman",
      "license_number": "D123456789",
      "phone_number": "0123456789"
    },
    "truck": {
      "truck_id": 1,
      "truck_number": "KL-1234-A",
      "model": "Isuzu NPR",
      "capacity_tons": 3.50
    },
    "customer": {
      "customer_id": 1,
      "company_name": "Genting Logistics Sdn Bhd",
      "contact_person": "Tan Wei Ming"
    },
    "pickup_location": "Port Klang Container Terminal",
    "delivery_location": "Genting Logistics Warehouse",
    "scheduled_date": "2024-06-20",
    "scheduled_time": "08:00:00",
    "status": "pending",
    "distance_km": 65.30,
    "estimated_duration": "02:30:00",
    "container_id": 1,
    "created_at": "2024-06-19T15:30:00Z"
  }
}
```

**Response (Error - 400 Bad Request):**
```json
{
  "status": "error",
  "message": "Driver or truck not available",
  "errors": {
    "driver_id": ["Driver is currently assigned to another schedule"],
    "truck_id": ["Truck is under maintenance"]
  }
}
```

### 2.2 API Endpoint 2: Delivery Status Update

**Endpoint URL:** `PUT /api/deliveries/{schedule_id}/status`

**Description:** Updates the delivery status and location tracking for an ongoing delivery.

**Request Headers:**
```
Content-Type: application/json
Authorization: Bearer {access_token}
```

**Request Body:**
```json
{
  "status": "in_progress",
  "current_location": {
    "latitude": 3.1390,
    "longitude": 101.6869,
    "address": "Kuala Lumpur City Centre"
  },
  "delivery_notes": "Traffic delay on PLUS highway, estimated 30 minutes late",
  "container_status": "in_transit",
  "estimated_arrival": "2024-06-20T10:30:00Z"
}
```

**Response (Success - 200 OK):**
```json
{
  "status": "success",
  "message": "Delivery status updated successfully",
  "data": {
    "schedule_id": 1,
    "status": "in_progress",
    "driver": {
      "driver_id": 1,
      "full_name": "Ahmad Hafiz bin Rahman",
      "phone_number": "0123456789"
    },
    "current_location": {
      "latitude": 3.1390,
      "longitude": 101.6869,
      "address": "Kuala Lumpur City Centre",
      "updated_at": "2024-06-20T08:45:00Z"
    },
    "delivery_progress": {
      "percentage_complete": 65,
      "distance_remaining_km": 22.80,
      "estimated_arrival": "2024-06-20T10:30:00Z"
    },
    "container": {
      "container_id": 1,
      "container_number": "CONT-001-2024",
      "status": "in_transit"
    },
    "customer_notification": {
      "notification_sent": true,
      "notification_time": "2024-06-20T08:45:00Z",
      "message": "Your delivery is in progress. Estimated arrival: 10:30 AM"
    },
    "delivery_notes": "Traffic delay on PLUS highway, estimated 30 minutes late",
    "last_updated": "2024-06-20T08:45:00Z"
  }
}
```

**Response (Error - 404 Not Found):**
```json
{
  "status": "error",
  "message": "Schedule not found",
  "error_code": "SCHEDULE_NOT_FOUND"
}
```

---

## 3. TEST CASE DESIGN

### Test Case 1: Driver Assignment Validation
**Test Case ID:** TC-001  
**Test Category:** Input Validation  
**Description:** Validate driver assignment to ensure only available drivers can be assigned to schedules.

**Input Data:**
```json
{
  "driver_id": 3,
  "truck_id": 1,
  "customer_id": 1,
  "scheduled_date": "2024-06-20",
  "scheduled_time": "08:00:00"
}
```

**Expected Output:**
```json
{
  "status": "error",
  "message": "Driver assignment validation failed",
  "errors": {
    "driver_id": ["Driver with ID 3 is currently on leave and not available for assignment"]
  }
}
```

**Validation Logic:**
- Check if driver exists in database
- Verify driver status is 'active'
- Ensure driver is not already assigned to another schedule on the same date/time
- Confirm driver has valid license

### Test Case 2: Container Weight Capacity Validation
**Test Case ID:** TC-002  
**Test Category:** Business Logic Validation  
**Description:** Validate that container weight does not exceed truck capacity.

**Input Data:**
```json
{
  "truck_id": 2,
  "container_weight_kg": 5000.00,
  "truck_capacity_tons": 2.80
}
```

**Expected Output:**
```json
{
  "status": "success",
  "message": "Container weight validation passed",
  "data": {
    "container_weight_kg": 5000.00,
    "truck_capacity_kg": 2800.00,
    "weight_utilization_percentage": 178.57,
    "validation_result": "EXCEEDS_CAPACITY"
  }
}
```

**Validation Logic:**
- Convert truck capacity from tons to kg
- Compare container weight with truck capacity
- Calculate utilization percentage
- Reject if weight exceeds 100% capacity
- Warning if weight exceeds 90% capacity

### Test Case 3: Billing Calculation Validation
**Test Case ID:** TC-003  
**Test Category:** Financial Calculation Validation  
**Description:** Validate accurate billing calculation including base rate, distance charges, and taxes.

**Input Data:**
```json
{
  "base_rate": 150.00,
  "distance_km": 65.30,
  "distance_rate": 2.50,
  "fuel_surcharge": 25.00,
  "tax_rate": 0.06
}
```

**Expected Output:**
```json
{
  "status": "success",
  "message": "Billing calculation completed",
  "data": {
    "base_rate": 150.00,
    "distance_charge": 163.25,
    "fuel_surcharge": 25.00,
    "subtotal": 338.25,
    "tax_amount": 20.30,
    "final_amount": 358.55,
    "calculation_breakdown": {
      "distance_charge": "65.30 km × RM 2.50 = RM 163.25",
      "tax_calculation": "RM 338.25 × 6% = RM 20.30"
    }
  }
}
```

**Validation Logic:**
- Calculate distance charge: distance_km × distance_rate
- Sum base_rate + distance_charge + fuel_surcharge = subtotal
- Calculate tax: subtotal × tax_rate
- Final amount: subtotal + tax_amount
- Round to 2 decimal places

### Test Case 4: Schedule Conflict Validation
**Test Case ID:** TC-004  
**Test Category:** Scheduling Validation  
**Description:** Validate that no scheduling conflicts occur when assigning drivers and trucks.

**Input Data:**
```json
{
  "driver_id": 1,
  "truck_id": 1,
  "scheduled_date": "2024-06-20",
  "scheduled_time": "08:00:00",
  "estimated_duration": "02:30:00"
}
```

**Expected Output:**
```json
{
  "status": "error",
  "message": "Scheduling conflict detected",
  "conflicts": {
    "driver_conflict": {
      "conflicting_schedule_id": 1,
      "scheduled_time": "08:00:00",
      "estimated_end_time": "10:30:00",
      "message": "Driver is already scheduled during this time period"
    },
    "truck_conflict": {
      "conflicting_schedule_id": 1,
      "scheduled_time": "08:00:00",
      "estimated_end_time": "10:30:00",
      "message": "Truck is already assigned during this time period"
    }
  }
}
```

**Validation Logic:**
- Check existing schedules for the same driver on the same date
- Calculate end time: scheduled_time + estimated_duration
- Check for overlapping time periods
- Verify truck availability during the scheduled time
- Consider buffer time between schedules (30 minutes)

---

## 4. DATABASE IMPLEMENTATION NOTES

### 4.1 Foreign Key Relationships
- **TRUCKS.driver_id** → **DRIVERS.driver_id**
- **SCHEDULES.driver_id** → **DRIVERS.driver_id**
- **SCHEDULES.truck_id** → **TRUCKS.truck_id**
- **SCHEDULES.customer_id** → **CUSTOMERS.customer_id**
- **CONTAINERS.truck_id** → **TRUCKS.truck_id**
- **CONTAINERS.schedule_id** → **SCHEDULES.schedule_id**
- **BILLING.customer_id** → **CUSTOMERS.customer_id**
- **BILLING.schedule_id** → **SCHEDULES.schedule_id**

### 4.2 Indexes for Performance
```sql
-- Performance indexes
CREATE INDEX idx_driver_status ON DRIVERS(status);
CREATE INDEX idx_truck_status ON TRUCKS(status);
CREATE INDEX idx_schedule_date ON SCHEDULES(scheduled_date);
CREATE INDEX idx_schedule_status ON SCHEDULES(status);
CREATE INDEX idx_container_status ON CONTAINERS(load_status);
CREATE INDEX idx_billing_status ON BILLING(payment_status);
```

### 4.3 Database Constraints
```sql
-- Additional constraints
ALTER TABLE SCHEDULES ADD CONSTRAINT chk_schedule_date 
CHECK (scheduled_date >= CURDATE());

ALTER TABLE CONTAINERS ADD CONSTRAINT chk_container_weight 
CHECK (weight_kg > 0);

ALTER TABLE BILLING ADD CONSTRAINT chk_billing_amounts 
CHECK (total_amount >= 0 AND final_amount >= total_amount);
```

---

**End of Database Design & Backend Documentation**
