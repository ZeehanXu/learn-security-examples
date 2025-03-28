# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Run the server **insecure.ts**.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.

Not enough repudiation because of insufficient logging. We cannot identify malicious users and investigate what they've done.

2. Briefly explain why the vulnerability is addressed in **secure.ts**.

The secure.ts version tackles repudiation by implementing centralized logging. Every request, critical event (like sending messages), and errors are logged with timestamps and client IPs. This audit trail makes it possible to trace user actions back to their source, helping investigators identify malicious activity and hold users accountable—even though further enhancement might still be needed.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

The secure version uses the Chain of Responsibility pattern via Express middleware.

Middleware Chain: Every incoming request is passed through a series of middleware functions. Each middleware (such as the logging middleware) processes the request (e.g., logging details) and then calls next() to delegate control to the next handler in the chain.
Centralized Logging: By placing logging middleware at the start of the chain, all requests and errors are consistently logged, creating a reliable audit trail for non-repudiation.
Error Handling: An error-handling middleware captures errors thrown by any middleware or route handler, logs the error, and sends an appropriate response without crashing the application.
