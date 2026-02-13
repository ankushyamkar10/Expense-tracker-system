# Expense Tracker System

A **microservices-based expense tracking system** built with an **event-driven architecture**, designed for **mobile-first, minimalistic expense management**.

The system separates concerns across independent services and integrates them using **Kafka** for asynchronous communication.

---

## Architecture Overview

- **Authentication Service** – Handles login and registration
- **User Service** – Manages user profiles
- **DS Service (LLM-powered)** – Extracts structured expense data (amount, merchant, currency)
- **Expense Service** – Stores and manages expense records
- **React Native Mobile App** – Minimal interface for tracking expenses

All services communicate asynchronously using **Kafka**.

---

## Project Structure

```bash
Expense-tracker-system/
├─ Auth-Service/        # Authentication API
├─ DS-Service/          # LLM extraction service (Kafka producer)
├─ Expense-Service/     # Expense management service (Kafka consumer)
├─ Expense-tracker/     # React Native mobile app
├─ User-Service/        # User account management service
└─ .gitmodules          # Git submodule definitions
```

---

## Backend Setup

### Prerequisites

Make sure the following services are running before starting the backend:

- ✅ Kafka
- ✅ MySQL

---

### Start Backend Services

Start each service independently:

#### Auth-Service

Handles:

- User registration
- User login
- Token management

#### User-Service

Handles:

- User profile management
- Account-related operations

#### DS-Service

- Uses LLM to extract structured data from raw expense input
- Publishes structured expense events to Kafka

Example extraction:

```
"Paid 450 INR at Starbucks"
→ { amount: 450, merchant: "Starbucks", currency: "INR" }
```

#### Expense-Service

- Subscribes to Kafka events
- Persists expense data into MySQL
- Provides APIs for expense history

---

## Running the Mobile App

Navigate to the mobile application directory:

```bash
cd Expense-tracker
npm install
npx react-native run-android   # For Android
# or
npx react-native run-ios       # For iOS
```

---

## Mobile App Features

- Login
- Add Expense
- View Expense History

Designed with a **minimal UI** for fast and distraction-free expense tracking.

---

## Event-Driven Flow

```
User Input (Mobile App)
        ↓
Auth-Service (Authentication)
        ↓
DS-Service (LLM Extraction)
        ↓
Kafka Event
        ↓
Expense-Service (Store in DB)
        ↓
Expense History API
        ↓
Mobile App UI
```

---

## Key Highlights

- Microservices Architecture
- Event-Driven Design (Kafka)
- Stateless Authentication
- LLM-powered Data Extraction
- Mobile-first UX
- Scalable & Decoupled Services

## Containerization (In Progress)

The system is currently being **dockerized** to ensure:

- Consistent development environments
- Faster onboarding
- Simplified deployments
- Better service isolation
- Production-ready infrastructure

Each microservice will run inside its own container.

### Planned Container Stack

- Auth-Service
- User-Service
- Expense-Service
- DS-Service
- Kafka + Zookeeper
- MySQL
- API Gateway

Once completed, the entire system can be started with:

```bash
docker-compose up --build
```

---

## API Gateway Integration (Next Step)

An **API Gateway** is being introduced to act as the single entry point for all client requests.

### Why API Gateway?

Instead of exposing multiple microservices:

```
Mobile App → Auth-Service
Mobile App → User-Service
Mobile App → Expense-Service
```

We centralize traffic:

```
Mobile App → API Gateway → Microservices
```

### Responsibilities of the Gateway

- ✅ Request routing
- ✅ Authentication & authorization
- ✅ Rate limiting
- ✅ Load balancing
- ✅ Centralized logging
- ✅ Improved security (services are not publicly exposed)

### Planned Architecture

```
                ┌─────────────┐
                │ Mobile App  │
                └──────┬──────┘
                       ↓
                ┌─────────────┐
                │ API Gateway │
                └──────┬──────┘
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
   Auth-Service   User-Service   Expense-Service
                        ↓
                     DS-Service
                        ↓
                       Kafka
```

---

## What This Enables

After Docker + Gateway integration, the system becomes:

✅ Cloud-ready  
✅ Horizontally scalable  
✅ Easier to deploy on AWS / Azure / GCP  
✅ Closer to real-world fintech architecture  
✅ Production-like for learning distributed systems

---
