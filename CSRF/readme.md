# Cross-Site Request Forgery (CSRF)

## Overview

**Steps**
1. User is logged in (example.com)
2. User visits an attacker's domain (evil.com)
3. User's data is changed (example.com)

**Consequences**  
changing user's email, password ,profile image ...

CSRF attack changes the **integrity** of the data in a user's account.

## Understanding of CSRF Attack
**Case 1: Change a user's email**  
1. User is logged in (example.com)
2. User visits an attacker's domain (evil.com)
3. Attacker tricks the user's browser into sending a request that changes the user's email (example.com)
4. Authentication cookie is appended to this request
5. User's email is changed (example.com)

The following code is host in the attacker's domain
```
<html>
<body>
Hello World
<script>
  var xhr = new XMLHttpRequest();
  xhr.open("POST", "https://example.com/profile.php", true);
  xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
  xhr.withCredentials = true;
  var body = "email=hacker@example.com&action=Change+email";
  xhr.send(body);
</script>
</body>
</html>
```
**Case 2: Takeover a user's account**
1. User is logged in (example.com)
2. User visits an attacker's domain (evil.com)
3. Attacker tricks the user's browser into sending two requests that changes the user's email and password (example.com)
4. Authentication cookie is appended to this request
5. User's account takeover (example.com)

**Defend**  
Append the anti-CSRF token to the request, e.g.  
email=hacker@example.com&action=Change+email&anti-csrf=eru3ift45dsfhdifv

## Validation of an Anti-CSRF Token
**Description**  
Attacker could bypass anti-CSRF token if there is no proper token validation implemented in the server-side.
1. The anti-CSRF token sent in a POST request.
2. Delete the anti-CSRF token from the request.
3. The request is accepted and processed by the web app.


## Login CSRF Attack
**Steps**  
1. User is logged in (example.com)
2. User visits an attacker's domain (evil.com)
3. Attacker tricks the user's browser into sending a login request with the attacker's email and password.
4. User is switched to the attacker's account.
5. User enters credit card data to the attacker's account.

**Defend**  
Append the anti-CSRF token to the login request, e.g.  
email=hacker@example.com&action=Change+email&anti-csrf=eru3ift45dsfhdifv

## Regeneration of Anti-CSRF Token

**Description**  
Attacker can launch an arbitrary CSRF token.
Make sure that the anti-CSRF token is regenerated at every time of authentication.

## Case Study

Form a CSRF attack to an unauthorized Admin Access

D-Link DIR-600 router
(hardware version: Bx; firmware version: 2.16)
Attacker can trick the router admin's browser into sending three requests:
1. request adds a new admin account(R/W access)
2. request enables remote management of the router
3. request causes a ping message to be sent from the router to the attacker's controlled machine



