# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

   ```
       http://localhost:3000/send-form
   ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

The code trusts attacker-controlled input (i.e., the userId from the request body) for authentication and authorization.
This can lead to privilege escalation or downgrading because an attacker might modify roles by supplying a manipulated ID.

2. Briefly explain how a malicious attacker can exploit them.

An attacker can submit a POST request to /update-role with an admin’s userId while setting newRole to a less privileged value (or vice versa), effectively downgrading or escalating privileges.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

Session Authentication:
The code verifies whether the user is logged in by checking for a valid session (using req.session.userId) rather than trusting user-supplied IDs.
