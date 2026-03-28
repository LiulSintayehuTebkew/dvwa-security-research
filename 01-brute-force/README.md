# DVWA Brute Force Vulnerabilities Documentation

## Overview

In this vulnerability we have login page asking username and password. Even though we know the username and password (username=admin,password=password) we pretend that we don’t know the credential and we try to brute force the password assuming.

---

## Security Level: Low

The first level of security is low. Which is very insecure interms of many things but one thing is that there is no input validation. This means it is also vulnerable to sql injection. I will write that in a different documentation.

---

## Key Insight

The power of brute force highly relies on the weakness of the password. If weak or common passwords are used brute forcing tools like burp suites intruder or hyda (for this demo I used Burp Suite, I will also write for hydra next time) it will be much easier and can get the correct password in minutes.

---

## Attack Demonstration (Using Burp Suite)

1. First we need to open burp suites built in browser and then go to the dvwa login page  
2. Now go to proxy and then turn on intercept  
3. Then click the web request and send it to the intruder  
4. Go to the intruder and select attack type then add position for payload  
5. Click load and put 100 common password list  
6. Start attack  
7. After some time sort the output by length and the different highest is the password  

---

## Takeaway

This vulnerability shows how dangerous weak passwords and lack of protections can be. Even a simple setup without rate limiting or input validation allows attackers to successfully brute force credentials in a short time.
