# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

   `npm install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

   ```
       <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
   ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

The session data (e.g., the "user" property) is unsanitized and directly inserted into HTML without escaping. An attacker who tampers with the session data could inject malicious HTML or JavaScript into the response, resulting in potential cross-site scripting (XSS) attacks.

2. Briefly explain how a malicious attacker can exploit them.

A malicious attacker can exploit the unsanitized session data injection by submitting specially crafted input (such as JavaScript code) through the registration form. When this input is stored in the session and later rendered in the HTML response without escaping, the injected script executes in the user's browser. This cross-site scripting (XSS) attack can lead to session hijacking, data theft, or redirection to malicious sites.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

secure.ts sanitizes user input using the escapeHTML function before storing it in the session. This prevents malicious HTML or JavaScript code from being injected into the response.
