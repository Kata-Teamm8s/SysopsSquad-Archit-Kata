This project is the solution for the SysopsSquad problem for Kata Teamm8s.

# Objective

Objective of the project is to save Penultimate Electronics, which struggles to keep its customers due to issues with the SysopsSquad ERP. The goal is to keep the user interfaces and the general workflows as is while enabling growth and improving user experience.

# Assumptions

To provide sufficient context for our solution, it's important to state our assumptions about the system as a starting point:

1. There are currently no known security issues. This entails that there is some reasonable authentication and authorization system in place and apps are expected to take full advantage of those.
2. The authentication and authorization layer is part of the monolith, there is no specific API gateway to speak of.
3. There is a feature-complete API layer which is used by both the Web and Mobile apps. Both Web and Mobile apps are simple representation layers.
4. There is only one database used by the system which was designed to be scaled vertically. (There may still be replicas in use.)
5. There are no read- or write-specific databases and/or database connections.
6. Monolith is running on a single, however large node, also designed for vertical scaling.
7. Static data (such as supported products and name-value pairs) does not live in the database, but in the server running the monolith.
8. All internal calls are synchronous and based on method calls. (Note that there is no programming language is assumed.)
9. There is a reasonable, strict CDP pipeline in place along with good software engineering practices in place. (Testing, static code analysis, etc.
10. Penultimate Electronics has a growing, but limited number of software engineers available. (20-30 engineers in 5-8 teams))
11. Session management is file based. (Typical setup in legacy PHP applications, but not limited to such.)

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
3. Ticketing Module (Ticket maintenance, Ticket routing, Ticket notification, Knowledge base maintenance)
4. Static Reference Module
5. Customer Module (Customer profile, Support contact, Billing)
6. Security Module (Login, Notification)

Are diagrams also mention a Routing layer, which may or may not be implemented as separate component inside the Monolith. If not a component, then it is composed as a specific part of each components.

# Architecture

## System Context

To provide better context for the system we're dealing with, we started with the [system context diagram](architecture/system-context-diagram.png), which is intended as both as the main reference point for all of the following diagrams for architects and engineers, and as for discussions with business owners. This diagram is expected to be expended over time, but not to otherwise change significantly, unless the company pivots its business.

## Containers

To further examine our situation, we then zoomed into the Sysops Squad ERP and crafted a [container diagram](architecture/container-diagram.png) with the individually deployable systems. Our assumption is that the Sysops Squad monolith consists of 4 main systems:

1. Sysops Squad Web App (frontend)
2. Sysops Squad Mobile App (frontend)
3. Sysops Squad API App (backend)
4. Sysops Squad DB (relational database)

Based on our assumptions - especially assumption #3: Both Web and Mobile apps are simple representation layers -, we then zoomed in into the Web API, and crafted our [component diagram](architecture/api-component-diagram.png) for that.

# Architectural characteristics

The architecture at hand is a monolith, showing issues at the time of project kick off. Each problem identified belongs in one of the following 3 main categories:

1. Availability
2. Elasticity
3. Performance

## Availability

- Fill out Survey (Customer)
- Read KnowledgeBase (Expert)
- Update KnowledgeBase (Expert)

## Elasticity

- Create ticket (Customer)
- Notify Expert of new task (System)
- Notify User of Expert being on their way (System)
- Notify User of new survey (System)
- Read ticket (Expert)
- Update ticket (Expert)

## Performance

- Generate reports (Admin / Manager)

## Further, less explicit issues

- Reliability (the system frequently “freezes up” or crashes)
- Maintainability (whenever a change is made, it takes too long and something else usually breaks)

# Proposed solution

Our solution consists of 3 distinct, but highly uneven steps:

1. Ensure solid foundations (small effort)
2. Save the company (medium effort)
3. Invest in the future (large effort)

## 1. Ensure solid foundations 

This phase mostly includes steps which are hard to represent in our C4 diagrams and should enable horizontal scaling of our servers. These changes are the following:

1. Move the static content inside the database. (ADR)
2. Move the sessions management into a key-value store. (ADR)
3. Introduce a load balancer.
4. Introduce read replicas of the Monolith database. (ADR)
5. Enable (manual) horizontal scaling of SysopsSquad ERP.

Once these steps are implemented, we have enabled a simple horizontal scaling of the SysopsSquad ERP which could technically. At this point our architecture will look like [this](proposal/solid-foundations.png)

## 2. Save the company

1. Extract the reporting sub-system. ([ADR](adrs/extract-reporting.md), [component diagram](proposal/reporting-extracted.png))
2. Extract the surveying sub-system. (ADR, [component diagram](proposal/surveying-extracted.png))
3. Asynchronous ticket routing. (ADR, [component diagram](proposal/async-ticket-routing.png))
4. Notification sub-system. ([ADR](adrs/notification-system.md), [component diagram](proposal/async-notification.png))

At this point all user-reported problems should be managed, but maintainability is expected to remain an issue.

## 3. Invest in the future

To address maintainability, we need to create a systems which can be easily modified. This requires components to be de-coupled and loosely coupled. As a starting point, we propose that modules no longer communicate with each other, except via accessing the databases and messaging services. In practice this means some version of a service-oriented architecture.

Once the engineering department grows much larger (100+), we expect to see a natural growth toward a microservices architecture, similar to our current [vision](proposal/vision.png), but which will be driven by organizational changes.


# List of Architecture Decision Records (ADRs)

1. Static Content in DB
2. Sessions in Key-Value Store
3. API Behind Load Balancers
4. Read Replicas for Read
5. [Extract The Reporting](adrs/extract-reporting.md)
6. [Notification System](adrs/notification-system.md)
7. Extract The Surveying System
8. Asynchronous ticket routing
