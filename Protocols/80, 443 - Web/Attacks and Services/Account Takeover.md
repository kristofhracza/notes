# Authorisation Issue

The email of an account should be attempted to be changed, and the confirmation process **must be examined**. If found to be **weak**, the email should be changed to that of the intended victim and then confirmed.

# XSS to Account Takeover
If there is [[XSS - Cross-Site Scripting]] present in the application, one might be able to steal session tokens or cookies.

# CSRF to Account Takeover
If one can exploit the website using [[CSRF - Cross Site Request Forgery]], they may be able to trick the user to change their password so they can access it.

# Host Header Injection
1. The Host header is modified following a password reset request initiation.
2. The `X-Forwarded-For` proxy header is altered to `attacker.com`.
3. The Host, Referrer, and Origin headers are simultaneously changed to `attacker.com`.
4. After initiating a password reset and then opting to resend the mail, all three of the aforementioned methods are employed.

# Response Manipulation
If the authentication response could be **reduced to a simple boolean just try to change false to true** and see if you get any access.


# CORS Misconfiguration to Account Takeover

If the page contains [[CORS - Misconfigurations & Bypass]] you might be able to **steal sensitive information** from the user to **takeover his account** or make him change auth information for the same purpose.


# References and more attacks
- [https://hacktricks.wiki/en/pentesting-web/account-takeover.html](https://hacktricks.wiki/en/pentesting-web/account-takeover.html)