# ⚙️ Consorcio
### Enterprise Management Platform · Microservices Architecture · Full Stack Project

---

## 🚀 Overview

**Consorcio** is a full-stack enterprise management platform developed as a Final Integrative Project (TPI) for the *Tecnicatura Universitaria en Programación* at Universidad Tecnológica Nacional (UTN), Facultad Regional Córdoba. The platform was built across three subjects: **Laboratorio de Computación IV**, **Programación IV**, and **Metodología de Sistemas**.

The system provides a unified solution for managing core business operations through three independent microservices:

- 📦 **Inventory management** — products, suppliers, warehouse movements and stock control
- 👥 **Human resources** — employees, attendances, positions and performance tracking
- 📋 **Contacts** — contact directory for users and administrators

The project was built with a strong focus on **modular architecture, domain separation, scalability, and professional-grade code quality**.

---

## 🧰 Tech Stack

### 🎨 Frontend
- **Angular 18** — Single Page Application consuming all backend services
- **Bootstrap** — Responsive UI components
- **DataTables** — Dynamic, sortable and paginated tables
- **jsPDF + jspdf-autotable** — PDF report generation
- **SheetJS (xlsx)** — Excel report generation
- **SweetAlert2** — User-friendly alert dialogs

### ⚙️ Backend
- **Java 17 + Spring Boot** — REST microservice implementation
- **Maven** — Dependency management and build tool
- **Swagger / OpenAPI** — API documentation for all services

### 🗄️ Database
- **Relational database** — Each service manages its own persistence layer

### 🧪 Testing
- **JUnit + Mockito** — Unit and integration testing

### 🛠️ Dev Tools
- **Git** — Version control, one repository per microservice
- **Jira Software** — Sprint planning and task tracking

---

## 🏗️ System Architecture

Consorcio follows a **microservices architecture** where each service owns a specific business domain and can be deployed, scaled and maintained independently. The Angular frontend consumes all three backend APIs.

```
┌────────────────────────────────────────────────────────┐
│                   Angular Frontend                     │
│         (Inventory · Employees · Contacts UI)          │
└───────────┬─────────────────┬────────────┬─────────────┘
            │ REST            │ REST       │ REST
   ┌────────▼────────┐  ┌─────▼──────┐  ┌─▼───────────┐
   │   Inventory     │  │  Employees │  │  Contacts   │
   │    Service      │  │   Service  │  │   Service   │
   └─────────────────┘  └────────────┘  └─────────────┘
```

Each service:
- 🔹 Owns its domain logic and data independently
- 🔹 Exposes a documented REST API
- 🔹 Can be deployed and tested in isolation
- 🔹 Follows consistent conventions across the platform

---

## 🧩 Services Breakdown

### 📦 Inventory Service
Manages the complete product and stock lifecycle.

**Responsibilities:**
- Product CRUD with categories, reusability flag and logical deletion
- Category management with discontinuation support
- Supplier management with filtering and logical deletion
- Warehouse movement registration (loans, returns, maintenance transfers)
- Stock modifications: increases with justification and supplier, logical decrements
- Movement search with multi-field filters (date, requestor, product, type)
- PDF report generation of active products

**Main Endpoints:**

| Method | Endpoint | Description |
|---|---|---|
| GET | `/product` | List all products |
| POST | `/product/product` | Create product |
| PUT | `/product/{id}` | Update product |
| PUT | `/product/{id}/logicalLow` | Logical deletion |
| GET | `/suppliers/all` | List all suppliers |
| POST | `/suppliers` | Create supplier |
| PUT | `/suppliers/bajalogica/{id}` | Logical deletion |
| POST | `/warehouseMovement` | Register movement |
| GET | `/warehouseMovement/search` | Search movements |
| POST | `/amountModification/modify-stock` | Increase stock |
| POST | `/amountModification/decrement` | Decrease stock |
| GET | `/category` | List categories |

📁 Repository: [2W1-TPI-inventory](https://github.com/LCIV-2024/2W1-TPI-inventory)

---

### 👥 Employees Service
Manages all human resources operations.

**Responsibilities:**
- Employee lifecycle: creation, update, activation/deactivation
- Multi-field employee profiles: personal data, address, working days and hours
- Third-party (outsourced) employee support with supplier linkage
- Position (charge) management with CRUD and status control
- Attendance tracking with state management (PRESENT, LATE, ABSENT, JUSTIFIED)
- Late arrival policy configuration
- Wake-up calls (performance warnings): individual and group creation
- Monthly performance evaluation per employee
- DNI and CUIL uniqueness validation

**Main Endpoints:**

| Method | Endpoint | Description |
|---|---|---|
| GET | `/employees/allEmployees` | List all employees |
| POST | `/employees/post` | Create employee |
| PUT | `/employees/put/{id}` | Update employee |
| GET | `/employees/validate/dni` | Validate DNI |
| GET | `/employees/validate/cuil` | Validate CUIL |
| POST | `/employees/configLateArrival` | Configure late arrival policy |
| GET | `/attendances/get` | List attendances |
| POST | `/attendances/post` | Register attendance |
| PUT | `/attendances/putState` | Update attendance state |
| POST | `/charges` | Create position |
| PUT | `/charges/{id}` | Update position |
| POST | `/wakeUpCalls/crear/grupo` | Group wake-up call |
| GET | `/wakeUpCalls/performance/mes` | Monthly performance |

📁 Repository: [2W1-TPI-employees](https://github.com/LCIV-2024/2W1-TPI-employees)

---

### 📋 Contacts Service
Centralized contact directory for end users and administrators.

**Responsibilities:**
- Contact CRUD with owner linkage
- Multi-criteria search (user ID, person type, contact type, value)
- Contact type catalog management
- Person type catalog management
- Role-based access: users manage their own contacts, admins have global visibility

**Main Endpoints:**

| Method | Endpoint | Description |
|---|---|---|
| POST | `/contact` | Create contact |
| POST | `/contact/owner` | Create contact with owner |
| PUT | `/contact/update` | Update contact |
| GET | `/contact/search` | Search contacts with filters |
| DELETE | `/contact/delete` | Delete contact |
| GET | `/contactType` | List contact types |
| GET | `/personType` | List person types |

📁 Repository: [2W1-TPI-contacts](https://github.com/LCIV-2024/2W1-TPI-contacts)

---

### 🎨 Frontend Application
Single Angular 18 application that integrates all three backend services into a unified UI.

**Key modules:**

- **Inventory module** — product listing with filters, stock increase modal, warehouse movement registration, movement search, stock history with date filters
- **Employees module** — employee list (info / attendances views), employee registration form, employee update form, position management, performance list with observation modal
- **Suppliers module** — supplier listing with search, supplier creation and update forms
- **Shared components** — reusable back button, filter component, DataTables integration, export to PDF/Excel across all modules

**Frontend capabilities:**
- DataTables with custom pagination, server-side search disabled (client filtering)
- Form validation with Angular template-driven and reactive forms
- Modal management with Bootstrap and custom overlay pattern
- Export to PDF (jsPDF) and Excel (SheetJS) on every listing view
- Dropdown action menus per row (edit, delete, view details)

📁 Repository: [2W1-TPI-frontend](https://github.com/TUP-FRC-UTN/tup-dabd-2w1-inventory-employees-providers.git) *(pending public access)*

---

## 🔁 Communication Pattern

| Type | Technology | Use Case |
|---|---|---|
| **Synchronous** | REST API | All frontend ↔ backend communication |
| **Inter-service** | REST calls | Cross-domain data (e.g., employees referenced in warehouse movements) |

Each service exposes a fully documented REST API consumable independently, making the system easy to integrate with future clients or services.

---

## 👤 Core Features

### 📦 Inventory
- Product catalog with category hierarchy and reusability tracking
- Supplier directory with type classification (inventory supplier, outsourced service, other)
- Full warehouse movement traceability (loans, returns, maintenance transfers)
- Stock increase with supplier and justification audit trail
- Logical decrement with justification
- Stock history with running balance calculation
- Export to PDF and Excel on all views

### 👥 Human Resources
- Complete employee profiles with personal, contract, address and schedule data
- Third-party employee management with supplier association
- Daily attendance tracking with state machine (PRESENT → LATE → ABSENT → JUSTIFIED)
- Configurable late arrival tolerance policy
- Performance monitoring with monthly aggregation
- Wake-up call system for individual and group performance warnings
- Position management with activation/deactivation

### 📋 Contacts
- Flexible contact model supporting multiple types per person
- Owner-based access control
- Admin-level global query capabilities

---

## 🎯 Architectural Decisions

| Decision | Rationale |
|---|---|
| Separate service per domain | Each team member could own a bounded context; independent deployability |
| Template-driven + reactive forms | Template-driven for simple forms, reactive for complex validation logic |
| Client-side DataTables filtering | Reduces backend load for small-to-medium datasets |
| Logical deletion throughout | Preserves data integrity and audit history across all entities |
| PDF + Excel export on every listing | Ensures every module meets reporting requirements independently |
| Single Angular app for all services | Reduces deployment complexity while keeping backend separation |

---

## 📌 Project Management

- **Methodology:** Agile — 2-week sprints
- **Academic year:** 2024
- **Subjects:** Laboratorio de Computación IV · Programación IV · Metodología de Sistemas

---

## 👥 Team

| Name |
|---|
| Abraham Loutaif, Agustín |
| Barrionuevo, Santiago Nahuel |
| Bon, Matías Nicolás |
| Caruso, Tomás |
| D'Angelo, Enzo Miguel |
| Herrado, Tomás Ezequiel |
| Nieto Luchini, Lázaro |
| Zea Cárdenas, Andrés Martín |

📍 Final Integrative Project (TPI) — Tecnicatura Universitaria en Programación  
Universidad Tecnológica Nacional · Facultad Regional Córdoba · 2024
