# authentication hacking

### SQL Injection
**Description**  
Hacker uses 'strange' username to bypass password validation and get unauthenticatedd access to user's account.

**Case Analysis**  
Suppose in webapp backend, the authentication logic is as follows.

Case 1: Correct SQL Syntax | Access to User's account
```
SELECT * FROM users WHERE email='alice@example.com' and password='abc'
```
If we add an extra ' to the SQL query, the backend will throw an execption and the user will be rejected to get access to the account.

Case 2: Incorrect SQL Syntax | No Access to User's account
```
SELECT * FROM users WHERE email='alice@example.com'' and password='abc'
```
However, if the hacker input a tricky username, it will comment out the password validation and the backend will throw no exception.

Case 3: Incorrect SQL Syntax | Access to User's account
```
SELECT * FROM users WHERE email='alice@example.com' -- ' and password='abc'
```
which is equivalent to 
```
SELECT * FROM users WHERE email='alice@example.com'
```

**Defend**  
Sanitization is a good way to defend the SQL  injection. By using the placeholder of variables, if the password is missing, it will throw exceptions, which make SQL injection fail.
```
dbQuery("SELECT * FROM users where email=? and password=?“);
dbBindParameters("ss", $email, $password);
dbStatementExecute();
```

### Dictionary Attack

### HTTPS Enforcement

### Session Regeneration

### User Enumeration

### Best Practices