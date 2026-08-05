# Product Requirement Document (PRD)
## Uber Eats / On-Demand Delivery Platform

**Version:** 1.0  
**Author:** Product Owner (PO)  
**Status:** Draft / Approved for Technical Architecture  

---

## 1. Executive Summary & Vision

The **Uber Eats On-Demand Delivery Platform** connects **Customers**, **Merchants** (restaurants/stores), and **Couriers** in real-time. The platform's primary goal is to provide a seamless, ultra-fast delivery experience by processing orders asynchronously, maintaining high data reliability, and matching nearby couriers with orders in real-time.

---

## 2. Target User Personas

| Persona | Primary Goal | Key Actions |
| :--- | :--- | :--- |
| **Customer** | Order food/groceries and receive them quickly. | Search merchants, place orders, pay, track courier live. |
| **Merchant** | Manage items and fulfill incoming orders efficiently. | Accept/reject orders, mark orders as preparing/ready. |
| **Courier** | Accept delivery jobs and earn income. | Go online/offline, receive dispatch offers, navigate to merchant & customer. |

---

## 3. Core Functional Requirements (Epics)

### Epic 1: Merchant & Menu Discovery (Search)
* **FR-1.1**: Customers must be able to search merchants by name, category, or menu item.
* **FR-1.2**: Customers must be able to filter merchants based on proximity (distance from delivery address).
* **FR-1.3**: Menu items and availability must be updated in near real-time.

### Epic 2: Order Placement & Checkout
* **FR-2.1**: Customers can add items to a Basket and proceed to Checkout.
* **FR-2.2**: The system must authorize payment before confirming the order.
* **FR-2.3**: Upon successful payment, an Order is created in state `CREATED` and an event is published to notify the Merchant.

### Epic 3: Merchant Order Fulfillment
* **FR-3.1**: Merchants receive incoming orders in real-time.
* **FR-3.2**: Merchants can accept the order (transitioning state to `PREPARING`).
* **FR-3.3**: Merchants mark the order as `READY_FOR_PICKUP` when preparation is complete.

### Epic 4: Automated Courier Dispatch & Tracking
* **FR-4.1**: Active Couriers emit GPS location updates every 3 seconds.
* **FR-4.2**: When an order reaches state `READY_FOR_PICKUP`, the Dispatch Engine selects the nearest available Courier.
* **FR-4.3**: Assigned Couriers accept the job, pick up the order, and transition state to `ON_THE_WAY`.
* **FR-4.4**: Customers can track the Courier's live location on a real-time map.

### Epic 5: Delivery & Settlement
* **FR-5.1**: Courier marks the order as `DELIVERED` upon physical handover.
* **FR-5.2**: The system completes financial settlement for Merchant and Courier earnings.

---

## 4. Non-Functional Requirements (NFRs)

### NFR-1: Reliability & Data Consistency
* **Zero Message Loss**: Every state transition in the Order lifecycle must emit an event without data loss (ensured via Transactional Outbox Pattern).
* **Eventual Consistency**: Microservices (Search, Dispatch, Notifications) must achieve eventual consistency within 1 second of an event being published.

### NFR-2: High Throughput & Low Latency
* **GPS Telemetry Ingestion**: The system must process high-frequency GPS pings from thousands of couriers without blocking database writes (handled via Redis Geo).
* **Search Response Time**: Merchant search queries must return results in under 100 milliseconds.

### NFR-3: Fault Tolerance & Resilience
* **Consumer Decoupling**: If the Notification or Search service goes down, Order creation and Courier dispatch must continue operating without interruption.