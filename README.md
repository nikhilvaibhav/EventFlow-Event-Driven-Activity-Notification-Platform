# EventFlow-Event-Driven-Activity-Notification-Platform

## ❓ What Problem EventFlow Solves

#backend #event-driven #scalability #reliability #low-latency #system-design

**EventFlow** is a backend platform where:

- Users perform actions (create journals, complete tasks)
- These actions emit events
- Other services react asynchronously to:
  - Build activity feeds
  - Send notifications

The system is designed for **scalability**, **reliability**, and **low latency**.

This mirrors real production systems in modern product companies.

## 🏗️ Architecture Style

#microservices #event-driven #polyglot-persistence #kafka #redis #docker #backend-architecture

- Pure microservices  
- Event-driven  
- Polyglot persistence  
- REST + Kafka (events)  
- Redis for performance and safety  
- Dockerized local environment

## 🧩 System Overview

**Total Number of Services:** 6

---

## 1️⃣ API Gateway

### Responsibility
- Single entry point  
- Request routing  
- Authentication validation  
- Rate limiting  

### Tech
- Spring Boot  
- Redis (rate limiting)  

### Database
- None  

---

## 2️⃣ Auth Service

### Responsibility
- User registration  
- Login  
- JWT issuance & validation  
- Token metadata  

### Database
- PostgreSQL  

### Tech
- Spring Security  
- Redis (login rate limit, token metadata)  

---

## 3️⃣ User Service

### Responsibility
- User profile management  
- Preferences  
- Account settings  

### Database
- PostgreSQL  

---

## 4️⃣ Action Service (Core Domain)

### Responsibility
- Journals  
- Tasks  
- Action lifecycle (create, update, complete)  
- Emits domain events  

### Database
- PostgreSQL  

### Events Emitted
- `action_created`  
- `action_completed`  

**This is the heart of the system.**

---

## 5️⃣ Notification Service

### Responsibility
- Consume events  
- Send notifications (email / push – mocked)  
- Retry & idempotency  

### Database
- MongoDB  

### Why Mongo
- High-write  
- Flexible schema  
- Append-heavy  

---

## 6️⃣ Activity Feed Service

### Responsibility
- Build user activity feeds  
- Read-optimized APIs  
- Fan-out processing  

### Database
- MongoDB  
- Redis (feed caching)  

---

## 📨 Messaging Backbone

**Kafka (or Redpanda – Kafka compatible)**

### Used For
- Asynchronous communication  
- Event propagation  
- Loose coupling  

### Key Topics
- `action_created`  
- `action_completed`  
- `user_registered`  

---

## ⚡ Redis Usage (Centralized but Logical)

- API rate limiting (Gateway)  
- Read-through caching (Feeds)  
- Idempotency keys (Kafka consumers)  
- Token metadata  

---

## 🗄️ Database Summary (Very Important)

| Service                 | Database     |
|-------------------------|--------------|
| API Gateway             | None         |
| Auth Service            | PostgreSQL  |
| User Service            | PostgreSQL  |
| Action Service          | PostgreSQL  |
| Notification Service    | MongoDB     |
| Activity Feed Service   | MongoDB     |

**This shows intentional polyglot persistence.**


