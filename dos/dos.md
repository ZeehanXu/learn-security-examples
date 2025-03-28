# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called **infodisclosure**. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

   ```
       http://localhost:3000/userinfo?id[$ne]=
   ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.

In insecure.ts, the query parameter (id) from the request is used directly in the MongoDB query without any validation or sanitization. This allows attackers to inject MongoDB operators (e.g., $ne), altering the intended query logic. Such injections can force the database to perform an unbounded or very heavy query operation.

2. Briefly explain how a malicious attacker can exploit them.

An attacker can send a request like:
http://localhost:3000/userinfo?id[$ne]=
This causes the query to become { \_id: { $ne: "" } }, which may trigger a full or inefficient collection scan. The resulting load can slow down or crash the server, leading to a Denial-of-Service (DoS) condition.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

The secure.ts version uses error handling to mitigate DoS risks by wrapping the database query in a try-catch block. This approach prevents Server Crashes: Even if a malformed or malicious query is injected, any exception is caught, ensuring that the server doesn’t crash.
