# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

   `$ npx install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

   `npx ts-node mal.ts`

4. Open **http://localhost:8000** in a browser, type a name and Submit.

5. Open the **Application** tab in the Browser's inspect pane. Find the **Cookies** under **Storage**. You should see a **connect.sid** cookie being set.

6. Open the HTML file **mal-steal-cookie.html** file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file **mal-csrf.html** file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out?

## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

   The vulnerability arises because the session cookie is configured with `httpOnly: false`. This means client-side scripts can read and modify the cookie. An attacker could use this to alter the session data—specifically, they might set the `user` property to `"Admin"` in their cookie. Once that happens, the attacker can access the `/sensitive` endpoint and trigger actions meant only for authenticated administrators.

2. Briefly explain different ways in which vulnerability can be exploited.

Direct Cookie Modification:
Since the cookie isn’t marked as httpOnly, an attacker can use the browser’s developer tools or injected scripts to modify the cookie value. They can change the user property to "Admin", gaining unauthorized access.

Cookie Theft via Malicious Script:
A crafted cross-site scripting (XSS) attack can read the cookie from document.cookie and send it to an attacker-controlled server. With the stolen cookie, the attacker can impersonate the user.

Cross-Site Request Forgery (CSRF):
A malicious site can capitalize on the ability to read or modify cookies to craft forged requests to the vulnerable server, potentially performing unauthorized actions without the user's knowledge.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

Secure.ts does not have the spoofing vulnerability because its session cookie is configured securely. In secure.ts, the cookie is set with httpOnly: true, which prevents client-side scripts from accessing or modifying the session cookie data. In addition, setting sameSite: true helps mitigate cross-site request forgery, further protecting the session data. These configurations ensure that attackers cannot easily manipulate the cookie to impersonate an admin user, unlike in insecure.ts where the cookie settings allowed possible spoofing.
