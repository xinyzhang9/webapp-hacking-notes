# Cookie Attacks
**Cookie Basic**  
Cookie: { Name, Value, Optional attributes }  
web app => browser ( set-cookie )  
browser => web app ( automatically appended )  

## Leakage of Cookie with Sensitive Data
Set-Cookie: name=value | send over HTTP and HTTPS  
Set-Cookie: name=value; secure | send over HTTPS only  


## Cookie Hijacking
Set-Cookie: name=value | XSS => cookie can be read  
Set-Cookie: name=value; HttpOnly | XSS => cookie can't be read  

## Weakness in Cookie Lifecycle
Make sure session id is regenerated so non-authenticated session id is different from authenticated session id.

Server-side invalidation must be done. For example,  
1. when user logged in, the cookie value is: _vcsd9y12hjbhjcv_ (authenticated session id)
2. when user logged out from browser, the cookie value is _zxc2112mvccvs_ (non-authenticated session id )  
3. Attacker copied _vcsd9y12hjbhjcv_ and refresh the page, the web app shows the user is logged in again, which means the user is not truly logged out.


## Understanding Risk: XSS via Cookie
**Overview**  
XSS via cookie != local exploitation  
a.example.com (XSS via cookie)  
Cross-Origin exploitation  
b.example.com (XSS) => a.example.com(XSS via cookie)

**Details**  
b.example.com(XSS)  
set cookie with domain=.example.com  
```
<script>
document.cookie='language\x3cscript\x3ealert("XSS")\x3c/script\x3e;domain=.example.com'
</script>
```
cookie sent to a.example.com  
a.example.com (XSS via cookie)  

**Lesson**  
Vulnerability on one sub-domain can be used to launch cookie-attack to another sub-domain.  

**Defend**  
```
<p> <script> alert("XSS")</script></p> // EXECUTED <script>...</script>
```
sanitize special characters
```
<p>&lt;script&gt; alert("XSS") &lt;/script&gt; </p> // DISPLATED <script>...</script>
```

## Remote Cookie Tampering
**Browser dependent exploitation may results in more powerful attack**  
Case analysis: comma-separated list of cookies (Safari)  
1. User is logged out. switch to mobile version by type ?version=mobile,SID=abc in the url.
2. When user is lured to visit the modified url, it automatically set two cookies version=mobile and SID=abc
3. When the user is logged in, the SID is overwritten by SID=abc, which was set by the attacker.
4. SID=abc becomes authenticated session id, and the attacker already know its value.

**Defend**  
If the only options of version is [desktop, mobile], then return a default value 'desktop' for a strange version value, such as version=xyz  

