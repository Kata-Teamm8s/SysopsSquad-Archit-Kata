# ADR: Extract the Notification System

## Status

PROPOSED

## Context

Notification mechanisms are used to notify users and experts about changes in the state of the ticket workflow. Currently, it's implemented as a component in the monolith system and uses external sms and email gateways.

The main concern is, provided with all the necessary data to generate notifications, to handle the communication with external gateways, and handle delays, timeouts and exceptions gracefully.

## Decision

1. Move notification component into a separate module.

2. The module should use no underlying data source.

3. The module should receive tasks via a messaging system, hence modules will be able to use the notification module in a "fire-and-forget" fashion.

## Consequences

1. We can extract the system without any major change and without data migration, therefore, the implementation risks are minimal.

2. It's easy to scale the notification system up and down on demand.

3. Communication with external gateways, retry policies, timeout and error handling are decoupled from the rest of the system, and, thus, increases overall system reliability.

4. The expected way of calling this module is async calls (fire-and-forget), so the overall performance of the ticketing flow is better since other modules should not wait for notification module responses.

5. Rejecting the use of a database requires other modules to provide all the necessary data explicitly (e.g. email address, phone number, customer personal data).

6. The overall system complexity slightly increases due to the natural overhead of distributed systems.
