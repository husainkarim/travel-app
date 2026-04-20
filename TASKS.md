# Tasks

## 🏗️ Phase 1: Infrastructure & Admin Foundation
*Focus: Environment, Automation, and the Admin Hub.*

### **1. Environment & DevOps (The "Factory")**
- [X] **Docker Orchestration:** Create `docker-compose.yml` for PostgreSQL, Neo4j, Jenkins, and SonarQube.
- [X] **Database Initialization:** Set up initial schemas for PostgreSQL (Users/Payments) and Neo4j (Travel Nodes).
- [ ] **Jenkins Pipeline:** Write a `Jenkinsfile` for automated Maven builds and SonarQube quality scans.
- [ ] **Ansible Basics:** Create a playbook to automate the deployment of your Docker containers.
- [ ] **Secret Management:** Deploy HashiCorp Vault and configure Spring Boot to pull DB credentials from it.

### **2. Backend (The "Engine")**
- [ ] **Security:** Implement Spring Security with **JWT** and Java 21 features (Records, Virtual Threads).
- [ ] **Admin API:** Build CRUD endpoints for Users, Travel Packages, and Payment methods.
- [ ] **Graph Logic:** Implement basic Neo4j repositories to link destinations and activities.
- [ ] **Payment Integration:** Set up the basic Stripe/PayPal SDK wrappers in your Java service.
- [ ] **Logging:** Configure the ELK stack (or Prometheus/Grafana) to capture logs from your services.

### **3. Frontend (The "Dashboard")**
- [ ] **Project Setup:** Initialize Angular project with a responsive layout (Tailwind/Material).
- [ ] **Auth Flow:** Build Login/Logout and an `AuthInterceptor` for JWT handling.
- [ ] **User Admin UI:** Build a table to List, Edit, and Delete users.
- [ ] **Travel Admin UI:** Build a multi-step form to create travel itineraries (Destinations, Dates, Accommodations).

---

## 🧠 Phase 2: Intelligence & Multi-Role Experience
*Focus: Search, Recommendations, and Role-Based Portals.*

### **1. Advanced Intelligence & Search**
- [ ] **Elasticsearch Integration:** Set up an Elasticsearch cluster and a sync mechanism (Logstash or Spring Data).
- [ ] **Search API:** Build a search endpoint with **Autocomplete** and **Fuzzy matching** for travels.
- [ ] **Neo4j Recommendations:** Write Cypher queries to suggest trips based on a user's past 3 fields (Category, Price, Region).
- [ ] **Role-Based Security:** Update Spring Security to enforce permissions for `ADMIN`, `MANAGER`, and `TRAVELER`.

### **2. Role-Specific Features**
- [ ] **Travel Manager Portal:** Build a dashboard showing "My Income" and "Subscriber Lists."
- [ ] **Traveler Portal:** Build "My Trips" view and the personal stats page (cancellations, preferred payments).
- [ ] **Payment Lifecycle:** Implement **Webhooks** to handle payment confirmation and trigger the "Subscribe" logic.
- [ ] **Business Rules:** Implement the "3-day cutoff" logic for cancellations and the Manager performance scoring.

### **3. UI/UX & Testing**
- [ ] **Responsive Design:** Ensure the Catalog and Booking pages work on Mobile (Chrome/Firefox).
- [ ] **Feedback System:** Add UI components for travelers to rate and report managers/trips.
- [ ] **Testing Suite:** - [ ] Unit Tests for Java logic (JUnit).
    - [ ] Integration Tests for Neo4j/ES (Testcontainers).
    - [ ] End-to-End (E2E) tests for the booking flow (Cypress).
- [ ] **Final Automation:** Update Ansible to handle the new Elasticsearch and ELK configurations.

---
