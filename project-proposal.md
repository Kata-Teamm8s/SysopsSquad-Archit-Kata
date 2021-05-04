# SysOps Squad Redesign Proposal

## Current Implementation

A monolith app, consising of User Interface, Business Logic and Database layers.

The main modules of the system are customer information, internal user management, login and notification, reporting and ticket related logic (includes ticket workflow logic, survey and knowledge base). 

The database has the following table groups: Knowledge Base, General Customer Data, Billing Data, Ticket Data, Expert Information and Survey Information. These table clusters are loosely connected between each other. There are no specific reporting tables.

## Issues

Company's customers reported issues with the ticket flow (inconsistent data due to missing and incorrectly handled tickets). Internal users reported that the system is unstable (freezes and crashes).

1. The system is fragile. Parallel request execution (e.g. customer or ticket creation, billing trigger and report generation) might lead to errors and timeouts.

2. The system is not elastic. In the case of increased usage, customer relevant data is lost (cusomer profile or ticket handling information).

3. Overall, the system is not easy to maintain. High coupled components lead to increased development costs and the high number of regression bugs.

## Redesign Goals

We define the following goals of system redesign:

1. Ensure basic best practices are enforced, enable horizontal scaling to save the company.

2. Look for low-hanging fruits, implement quick wins, to save time and enable growth until larger refactorings can happen.

3. Since the customer data and the ticket flow are the most valuable business artifacts for SysOps Squad, try to move these features into the center of our new architecture and guarantee high reliability of these components.

4. Make the system stable. Issues in one part of the system should not affect the other parts.

5. Increase system's elasticity. Parts of the system that are expected to experience bursts of requests should be easy to scale up and down on demand.

6. Decrease code coupling and increase cohesion. Use the current implementation as a reference for feature clustering.


## Risks

1. Redesign / refactoring should be conducted without negative impact on customer experience. Client behaviour stays exactly the same as before redesign. 

2. If needed, we can consider a possible downtime to execute data migration, because we do not expect active system usage during night hours, since the system is used only within one country. 

3. In order to minimze the development costs, the legacy code should be reused as much as possible.