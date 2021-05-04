This project is the solution for the SysopsSquad problem for Kata Teamm8s.

# Requirements

Requirements of the project is to save Penultimate Electronics. The goal is to keep the user interfaces and the general workflows as is while enabling growth and improving user experience.

# Assumptions

To provide sufficient context for our solution, it's important to state our assumptions about the system as a starting point:

1. There are currently no known security issues. This entails that there is some reasonable authentication and authorization system in place and apps are expected to take full advantage of those.
2. The authentication and authorization layer is part of the monolith, there is no specific API gateway to speak of.
3. There is a feature-complete API layer which is used by both the Web and Mobile apps.
4. There is only one database used by the system which was designed to be scaled vertically. (There may still be replicas in use.)
5. There are no read- or write-specific databases and/or database connections.
6. Static data (such as supported products ad name-value pairs) does not live in the database, but in the filesystem.
7. All internal calls are synchronous and based on method calls.
8. There is a reasonable, strict CDP pipeline in place along with good software engineering practices in place.

Note that there is no programming language is assumed.

# Actors

The SysopsSquad System has 3 internal and 1 external actors, plus the system itself:
1. Customer (External)
2. Administrator (Internal)
3. Expert (Internal)
4. Manager (Internal)
5. System

# Components

The SysopsSquad System has the following components (and responsibilities):
1. Export Module (Expert profile, User maintenance)
2. Reporting Module (Reporting Shared, Financial reports, Ticket reports, Expert reports)
3. Ticketing Module (Ticket maintenance, Ticket routing, Ticket notification, Knowledge base maintenance, Survey Module)
4. Static Reference Module
5. Customer Module (Customer profile, Support contact, Billing)
6. Security Module (Login, Notification)

# Architectural characteristics

The architecture at hand is a monolith, showing issues at the time of project kick off. Each problem identified belongs in one of the following 3 categories:
1. Availability
2. Elasticity
3. Performance

## Availability:
- Fill out Survey (Customer)
- Read KnowledgeBase (Expert)
- Update KnowledgeBase (Expert)

## Elasticity:
- Create ticket (Customer)
- Notify Expert of new task (System)
- Notify User of Expert being on their way (System)
- Notify User of new survey (System)
- Read ticket (Expert)
- Update ticket (Expert)

## Performance:
- Generate reports (Admin / Manager)

# Proposed solution, iteration 1

- extract services
- database decomposition

# Migration plan (if any)

# ADRs list

1. [Extract Reporting](adrs/extract-reporting.md)
2. [Notification System](adrs/notification-system.md)
