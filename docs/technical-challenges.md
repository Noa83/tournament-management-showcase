# Technical Challenges & Engineering Decisions

## Overview

The AnnecyVB Tournament Management Platform addresses several technical challenges involved in managing a sports tournament.

Beyond standard CRUD operations, the application handles concurrent registrations, online payments, sensitive personal information, background processing, and complex event administration.

This document highlights selected engineering challenges and the technical mechanisms implemented to address them.

The project is collaborative. The examples describe the application's architecture and do not imply sole authorship of every component.

---

## 1. Managing Concurrent Registrations

### The Challenge

The tournament brings together more than **550 teams, representing approximately 1,800 players**. Registration capacity is typically reached in less than one hour after registration opens, concentrating a significant volume of requests into a very short period.

This creates concurrency challenges and requires reliable registration processing, particularly to prevent available capacity from being exceeded.

The application incorporates an access queue to regulate incoming traffic, along with database transactions and pessimistic locking mechanisms to protect critical registration and payment operations.

### Technical Approach

The application implements several complementary mechanisms:

**Registration Access Queue**

An application-level queue regulates access to the registration process.

The implementation uses:

- Session-based access tracking.
- Configurable active-session limits.
- Session expiration and renewal.
- File-based state persistence.
- File locking (`flock`) for concurrent state updates.

**Registration Throttling**

Additional controls limit registration attempts based on configurable thresholds.

**Database-Level Concurrency Control**

During payment confirmation, Doctrine pessimistic locking is used to coordinate operations affecting payment records, teams, and registration capacity.

### Engineering Considerations

The file-based access queue is designed around shared local filesystem state.

A distributed deployment would require reviewing this implementation and potentially replacing it with a shared storage mechanism.

### Key Technical Concepts

Concurrency control · PHP file locking · Session management · Doctrine transactions · Pessimistic locking

---

## 2. Payment Processing & Data Consistency

### The Challenge

Online payment processing involves interactions between the application, an external payment gateway, and the database.

The application must coordinate payment confirmation with the corresponding team registration state.

### Technical Approach

The platform integrates the Monetico payment gateway.

The payment confirmation flow follows a synchronous request-processing model:

1. The application initiates the payment request.
2. The payment gateway processes the transaction.
3. A payment confirmation callback is received by the Symfony application.
4. The application validates and processes the response.
5. The payment service updates the relevant application state using a database transaction.

The implementation uses Doctrine transactions and pessimistic locking.

Exceptions during transactional processing trigger rollback handling.

### Engineering Considerations

Payment-related operations must account for concurrent requests and repeated confirmation attempts.

The application's handling of these scenarios should be evaluated through dedicated automated tests.

### Key Technical Concepts

Payment gateway integration · HTTP callbacks · Transaction management · Data consistency · Error handling

---

## 3. Personal Data Protection

### The Challenge

Tournament registration requires collecting personal information about participants.

The application must restrict access to sensitive information and protect selected data fields.

### Technical Approach

The application implements several mechanisms:

**Field-Level Encryption**

Selected personal data fields are encrypted using AES-256-GCM.

The implementation uses randomly generated initialization vectors and authentication tags.

**Email Processing**

HMAC-SHA256 is used for certain operations involving normalized email addresses.

**Access Control**

Symfony Security provides authentication and role-based access restrictions for administrative and participant functionality.

### Engineering Considerations

Application security requires reviewing the entire data lifecycle, including storage, access permissions, key management, logging, and exports.

The presence of encryption and access-control mechanisms does not replace a full security audit.

### Key Technical Concepts

AES-256-GCM · HMAC-SHA256 · Symfony Security · Data confidentiality · Access control

---

## 4. Background Processing & Document Generation

### The Challenge

Some operations can require more processing time than standard HTTP requests.

Examples include generating documents, preparing match sheets, and sending email campaigns.

### Technical Approach

Symfony Messenger provides an asynchronous processing mechanism.

The application uses:

- Dedicated message classes.
- Message handlers.
- A configurable asynchronous transport.
- A background worker.
- A failure transport for unsuccessful jobs.

Background operations include selected PDF generation tasks, team invoice generation, match sheet generation, and email campaigns.

### Engineering Considerations

Asynchronous processing introduces additional considerations:

- Retry and failure handling.
- Duplicate message processing.
- Monitoring of background workers.
- Data consistency across execution boundaries.

These areas are important when evolving the application's reliability and deployment architecture.

### Key Technical Concepts

Symfony Messenger · Message handlers · Background workers · Asynchronous processing · Failure handling

---

## 5. Complex Tournament Scheduling

### The Challenge

Tournament scheduling requires coordinating teams, competition categories, available courts, and successive competition phases.

The application includes a dedicated scheduling engine that handles these constraints.

### Development Attribution

The core scheduling algorithm was primarily designed and developed by **Loup**, another contributor to the project.

My involvement in this component has been limited to minor adjustments and occasional maintenance.

The scheduling engine is included here to explain the application's technical complexity, not as a claim of personal algorithm design.

### Technical Characteristics

The implementation includes:

- Team grouping.
- Court allocation.
- Match generation.
- Tournament progression.
- Knockout-stage generation.
- Simulation capabilities.

### Engineering Considerations

The scheduling service contains extensive business logic and maintains substantial internal state.

Additional automated testing and a clearer separation of calculation and persistence responsibilities could improve future maintainability.

---

## 6. Maintaining and Evolving a Long-Term Application

### The Challenge

An application used over multiple tournament editions must evolve alongside changing operational requirements.

New features need to coexist with existing registration, payment, and competition workflows.

### Technical Approach

The project uses a Symfony application structure with dedicated services, entities, repositories, controllers, and database migrations.

Docker provides a reproducible development environment.

The project has evolved through successive development iterations since 2021.

### Engineering Considerations

Long-term maintainability remains an important area of improvement.

Potential areas include:

- Expanding automated test coverage.
- Introducing continuous integration checks.
- Reducing tightly coupled business logic.
- Reviewing security-sensitive endpoints.
- Improving front-end consistency and maintainability.

These items represent improvement opportunities rather than completed implementations.

---

## Future Development

The platform is functional within its current scope.

Planned enhancements include:

- A player-facing match score submission interface.
- A pre-registration and randomized team selection workflow.
- A front-end redesign.
- Further improvements based on user and organizer feedback.

---

## Source Code Confidentiality

The application source code remains private within the AnnecyVB GitHub organization.

This case study describes architectural concepts and implementation approaches without publishing proprietary source code or confidential information.