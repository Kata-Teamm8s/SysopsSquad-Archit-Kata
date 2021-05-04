This project is the solution for the SysopsSquad problem for Kata Teamm8s.

# Requirements

Requirements of the project is to save Penultimate Electronics. The goal is to keep the user interfaces and the general workflows as is while enabling growth and improving user experience.

# Assumptions

To provide sufficient context for our solution, it's important to state our assumptions about the system as a starting point:

1. There are currently no known security issues. This entails that there is some reasonable authentication and authorization system in place and apps are expected to take full advantage of those.
2. The authentication and authorization layer is part of the monolith, there is no specific API gateway to speak of.
3. There is a feature-complete API layer which is used by both the Web and Mobile apps. Both Web and Mobile apps are simple representation layers.
4. There is only one database used by the system which was designed to be scaled vertically. (There may still be replicas in use.)
5. There are no read- or write-specific databases and/or database connections.
6. Static data (such as supported products ad name-value pairs) does not live in the database, but in the filesystem.
7. All internal calls are synchronous and based on method calls.
8. There is a reasonable, strict CDP pipeline in place along with good software engineering practices in place. (Testing, static code analysis, etc.)
9. We have a growing, but limited number of software engineers available. (20-30 engineers in 5-8 teams)

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

# Architecture

To provide better context for the system we're dealing with, we started with the [system context diagram](architecture/system-context-diagram.png), which is intended as both as the main reference point for all of the following diagrams for architects and engineers, and as for discussions with business owners. This diagram is expected to be expended over time, but not to otherwise change significantly, unless the company pivots its business.

To further examine our situation, we then zoomed into the Sysops Squad ERP and crafted a [container diagram](architecture/container-diagram.png) with the individually deployable systems. Our assumption is that the Sysops Squad monolith consists of 3 main systems:

1. Sysops Squad Web App
2. Sysops Squad Mobile App
3. Sysops Squad API App

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

# Migration plan (if any)



# List of Architecture Decision Records (ADRs)

1. [Extract Reporting](adrs/extract-reporting.md)
2. [Notification System](adrs/notification-system.md)
