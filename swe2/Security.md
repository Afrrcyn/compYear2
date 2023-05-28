We need to ask some questions first:
- We need to know what are our assets? - Assets are the thing we want protect.
- Who do we want to protect against?
- How can our assets get attacked? - What vulnerabilities do we have?

>[!bug] OWASP 10 Security Risks
>1. Injection
>2. Broken Authentication
>3. Sensitive Data Exposure
>4. XML External Entities
>5. Broken Access Control
>6. Security Misconfiguration
>7. Cross-site Scripting
>8. Insecure Deserialisation
>9. Using components with known vulnerabilities 
>10. Insufficient Logging and Monitoring


When authenticating a user we use credentials - and there are different types of credentials :

- Who you are:
	- Biometrics
	- Fingerprints
	- Face Recognition 
- What you know:
	- Username And Password
	- PIN
- What you have:
	- USB Token
	- One-time Password


Salt and Hash Passwords!

Example Hash functions:
- MD5 - insecure
- SHA1 - not recommended
- SHA2 and SHA3 - secure

Example password hash functions:
- bcrypt
- scrypt
- Lyra2
- Argon2
(All secure)

Authorisation is the process of specifying access rights.


Injections occur when attacks inject untrusted inputs which get processed as part of a command or query.

Common Injection attacks: Cross-Sit Request Forgery (CSRF) Cross-Site Scripting (XSS) and SQL Injection

goto: [[Privacy Design Strategies|Privacy Reading]], [[web_application_security_engineering.pdf|Security Reading]], [[SWE 2 Home Page|Home Page]]