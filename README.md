# GearLog: Smart Vehicle Lifecycle & Predictive Maintenance Engine

[![Java](https://img.shields.io/badge/Java-17%2B-ED8B00?style=flat&logo=openjdk&logoColor=white)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-6DB33F?style=flat&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?style=flat&logo=flutter&logoColor=white)](https://flutter.dev/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15%2B-316192?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> **GearLog** is a comprehensive vehicle asset management platform that tracks real-time component wear using dual-metric predictive modeling, digitizes workshop invoices, automates statutory document reminders, and generates tamper-resistant Digital Service Passports for transparent vehicle resale.

---

## 📌 Problem Overview

Vehicle owners often rely on perishable windshield stickers, scattered paper receipts, or guesswork to track scheduled servicing. Consequently:
- Critical preventative maintenance (oil flushes, brake pads, timing chains) is neglected, precipitating catastrophic mechanical failures and inflated repair expenses.
- Paper receipts issued by independent service stations are routinely misplaced, leaving vehicle owners without an audit trail for warranty claims.
- The secondary automotive marketplace suffers from an asymmetric information deficit—buyers have no verifiable, fraud-resistant proof of previous upkeep or actual odometer progression.

---

## 💡 Key Architectural Capabilities

### 1. Dual-Metric Predictive Degradation Engine
Component degradation is calculated across two independent vectors: logged distance elapsed and calendar duration. Wear thresholds evaluate whichever parameter expires first:

```math
\text{Wear}_{\text{distance}} = \left(\frac{\text{Odometer}_{\text{current}} - \text{Odometer}_{\text{last\_service}}}{\text{Interval}_{\text{km}}}\right) \times 100
```

```math
$$\text{Wear}_{\text{time}} = \left(\frac{\text{Days Since Last Service}}{\text{Interval}_{\text{days}}}\right) \times 100$$
```

```math
$$\text{Current Wear \%} = \max(\text{Wear}_{\text{distance}}, \text{Wear}_{\text{time}})$$
```

Using historical daily odometer velocity, the system projects precise forecast dates for critical service milestones.

### 2. Transferable Digital Service Passport
- **Fraud Mitigation:** Enforces strict temporal odometer validation ($\text{Odometer}_t \ge \text{Odometer}_{t-1}$) to prevent odometer tampering.
- **Public Audit Token:** Generates a read-only, cryptographically masked inspection URL/QR code allowing prospective buyers to review verified service stamps without revealing owner PII.
- **PDF Dossier Export:** Compiles a complete vehicle lifecycle dossier containing maintenance history, cost allocations, and verified workshop receipts.

### 3. Statutory Compliance Sentinel
Automated background task runners monitor revenue license renewals, insurance policy schedules, and emission testing certifications, dispatching scheduled alerts at 30, 14, 3, and 0-day thresholds.

### 4. Total Cost of Ownership (TCO) Ledger
Granular financial classification distinguishing scheduled servicing, unplanned mechanical repairs, wear-and-tear consumables, and statutory expenses to compute actual operational cost-per-kilometer ($\frac{\text{Expenses}}{\text{Distance}}$).

---

## 🛠 Tech Stack

| Domain | Technology | Description |
| :--- | :--- | :--- |
| **Mobile Client** | Flutter / Dart | Cross-platform mobile client with local offline caching and camera receipt scanning. |
| **Backend API** | Spring Boot 3 (Java 17/21) | Layered enterprise REST API, Spring Security, stateless JWT authentication. |
| **Persistence** | PostgreSQL | Relational schema with spatial indexing and foreign-key cascading constraints. |
| **Task Scheduling** | Spring `@Scheduled` / Quartz | Automated cron workers evaluating document expiration dates and threshold alerts. |
| **Object Storage** | AWS S3 / Firebase Storage | Scalable cloud bucket storage for digitized receipt scans and inspection records. |
| **Document Generation** | OpenPDF / iText | Dynamic server-side compilation of Digital Service Passports. |

---

## 📂 Repository Layout

```text
gearlog/
├── backend/             # Spring Boot 3 REST Application
│   ├── src/main/java/   # Controllers, Domain Entities, Repositories, Services
│   └── src/main/resources/
├── mobile/              # Flutter Cross-Platform Client
│   ├── lib/             # Presentation Layer, State Management, Repositories
│   └── assets/          # Icons, Graphics, and Localization resources
├── docs/                # Database Schemas, UML Class/Sequence Diagrams, API Specs
├── .gitignore
├── LICENSE
└── README.md
