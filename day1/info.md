# Advent of Code Day 1
## IDOR Vulnerabilities

- **How to find them**
1. **Query Component:**

Query component data is passed in the URL when making a request to a website. 
Take, for instance, the following screenshot of an URL.


We can breakdown this URL into the following:

- Protocol: https://

- Domain: website.thm

- Page: /profile

- Query Component: id=23

2. **Post Variables**
Examining the contents of forms on a website can sometimes reveal fields that could be vulnerable to IDOR exploitation. Take, for example, the following HTML code for a form that updates a user's password.
```
<form method="POST" action="/update-password">
    <input type="hidden" name"user_id" value="123">
    <div>New Password:</div>
    <div><input type="password" name="new_password"></div>
    <div><input type="submit" value="Change Password">
</form>
```
user's id is being passed to the webserver in a hidden field. 
Changing the value of this field from 123 to another user_id 
may result in changing the password for another user's account.

3. **Cookies**

To stay logged into a websites cookies are used to remember your session. 
Usually, this will involve sending a session id which is a long string of 
random hard to guess text such as 5db28452c4161cf88c6f33e57b62a357, 
which the webserver securely uses to retrieve your user information and 
validate your session. Sometimes though, less experienced developers may 
store user information in the cookie its self, such as the user's ID. 
Changing the value of this cookie could result in displaying another 
user's information. See below for an example of how this might look.

```
GET /user-information HTTP/1.1
Host: website.thm
Cookie: user_id=9
User-Agent: Mozilla/5.0 (Ubuntu;Linux) Firefox/94.0

Hello Jon!

GET /user-information HTTP/1.1
Host: website.thm
Cookie: user_id=5
User-Agent: Mozilla/5.0 (Ubuntu;Linux) Firefox/94.0

Hello Martin!
```

* Example bug bounty URL: *
https://corneacristian.medium.com/top-25-idor-bug-bounty-reports-ba8cd59ad331

* Suggested room for learning more about IDOR vulns *
https://tryhackme.com/room/idor
