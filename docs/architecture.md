# Technical Architecture

## Overview

The Tournament Management Platform is a PHP/Symfony application developed to support the organization and operation of the Annecy Volleyball Pentecost Tournament.

The application combines several business domains, including participant registration, payments, competition management, volunteer coordination, and document generation.

Its architecture is organized around Symfony controllers, dedicated business services, Doctrine entities and repositories, and asynchronous message handlers.

> This document provides a high-level overview of the existing architecture. The application source code remains private.

## Architecture Overview

The following diagram illustrates the application's technical architecture, including its main functional modules, data persistence, external integrations, and background processing.

![Tournament Management Platform — Technical Architecture](../assets/architecture.png)

*Technical architecture of the AnnecyVB Tournament Management Platform. Payment confirmation is handled synchronously, while selected document generation and email operations are processed asynchronously through Symfony Messenger.*

---

## 1. Technology Stack

| Layer | Technology |
|---|---|
| Programming language | PHP 8.2+ |
| Application framework | Symfony 7.4 |
| Object-relational mapping | Doctrine ORM |
| Database | Relational database configured through Doctrine DBAL |
| Templates | Twig |
| Front-end | JavaScript, CSS, Webpack Encore |
| Background processing | Symfony Messenger |
| Payment gateway | Monetico |
| Authentication | Symfony Security |
| Development infrastructure | Docker, Nginx, PHP-FPM |

## 2. Application Organization

The application uses a modular organization within the Symfony project.

### Controllers

Controllers handle HTTP requests, form submissions, and interactions with the business layer.

They cover several functional areas:

- Participant and team registration.
- Competition administration.
- Match results and rankings.
- Payment processing.
- Volunteer and event administration.
- PDF generation and document access.

### Business Services

Dedicated services implement application-specific business operations.

These include:

- Registration access management.
- Payment processing and confirmation.
- Tournament scheduling.
- Team administration.
- Volunteer and event management.
- Personal data protection.
- Document generation.

### Persistence Layer

Doctrine ORM provides object-relational mapping and database access.

Entities represent the principal business objects, including participants, teams, matches, payments, courts, volunteers, and competition results.

Repositories encapsulate database queries and persistence-related operations.

## 3. Registration and Payment Processing

Registration management combines several mechanisms.

**Access control**

An application-level queue regulates access to the registration process.

Its current implementation relies on file-based state management and locking.

**Capacity management**

The application controls registration capacity according to tournament rules and configuration.

**Payment confirmation**

The payment workflow integrates Monetico.

Database transactions and pessimistic locking are used during payment confirmation to coordinate changes to payment and team registration states.

These mechanisms help manage situations involving concurrent requests.

## 4. Tournament Scheduling

The application includes a scheduling engine responsible for organizing tournament encounters.

It handles operations related to:

- Team grouping.
- Competition categories.
- Match generation.
- Court allocation.
- Tournament progression.
- Knockout rounds.

**Development attribution**

The core scheduling algorithm was primarily designed and developed by Loup.

My contributions to this component have been limited to minor adjustments and occasional maintenance.

The algorithm is described here as an important architectural component of the application, rather than as one of my primary personal achievements.

## 5. Asynchronous Processing

Symfony Messenger handles several background operations.

The application's configuration routes messages for operations including:

- PDF generation.
- Team invoice generation.
- Match sheet generation.
- Email campaigns.
- Selected registration processing operations.
- Test-data and score generation.

A dedicated worker consumes asynchronous messages.

The development configuration uses a Doctrine-backed Messenger transport.

## 6. Security and Data Protection

The application uses Symfony Security for authentication and role-based access restrictions.

Certain personal data fields are protected using AES-256-GCM encryption.

HMAC-SHA256 is used for selected email-related processing.

Additional mechanisms include registration throttling and access restrictions for administrative features.

These mechanisms describe the existing implementation and should not be interpreted as a certification of application security.

## 7. Development Environment

The Docker Compose configuration defines three application services.

**Nginx**

Provides the HTTP entry point.

**PHP-FPM**

Runs the Symfony application.

**Messenger Worker**

Processes asynchronous messages.

The database connection is provided through application configuration rather than through an active database service in the current Docker Compose definition.

## 8. Planned Evolution

The current application is fully functional within its existing scope.

The following improvements are planned or under consideration:

### Player Score Submission

A dedicated interface allowing players to submit match results, with appropriate verification and validation workflows.

### Pre-Registration and Random Selection

A new workflow combining pre-registration, eligibility rules, randomized team selection, and confirmation.

### User Interface Redesign

A modernization of the front-end to improve usability, accessibility, responsive behavior, and visual consistency.

### Continuous Improvement

Further enhancements driven by operational needs and user feedback.

## 9. Source Code Confidentiality

This architecture overview is intended for a public technical portfolio.

The original application and its source code remain private within the AnnecyVB GitHub organization.

This document does not reproduce proprietary source code, credentials, private configuration values, or participant data.