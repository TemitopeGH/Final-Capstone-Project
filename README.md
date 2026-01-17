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
What are five remediation methods for preventing SQL injection exploits? <br>
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
If not already, log in to the server at 10.5.5.12 with the admin/password credentials.
### Step 2: From the results of your reconnaissance, determine which directories are viewable using a web browser and URL manipulation. ###
Perform reconnaissance on the server to find directories where indexing was found using **nikto -h 10.5.5.12** <br>
<img width="1366" height="702" alt="nikto -h ip address" src="https://github.com/user-attachments/assets/fde0290d-e362-4062-aaed-67e3b0f95eee" />

Which directories can be accessed through a web browser to list the files and subdirectories that they contain? <br>
**+ /config/: Directory indexing found.** <br>
**+ /docs/: Directory indexing found.** <br>
### Step 3: View the files contained in each directory to find the file containing the flag. ###
Create a URL in the web browser to access the viewable subdirectories. Find the file with the code for Challenge 2 located in one of the subdirectories. <br>
We use 10.5.5.12/config <br>

<img width="1366" height="702" alt="Screenshot_2026-01-17_11_45_58" src="https://github.com/user-attachments/assets/b6ee6c0d-3d43-4766-baf1-5715f1a8b15b" />
In which two subdirectories can you look for the file? <br>
**+ /config/: Directory indexing found.** <br>
**+ /docs/: Directory indexing found.** <br>
What is the filename with the Challenge 2 code? <br>
the file with the code for challenge 2 is **db_form.html** <br>
Which subdirectory held the file? <br>
**Parent Directory** held the file <br>
What is the message contained in the flag file? Enter the code that you find in the file. <br>
<img width="1366" height="702" alt="great work challenge 2 code" src="https://github.com/user-attachments/assets/868508a6-e35f-4ba7-884e-c5d73aa7c764" />

### Step 4: Research and propose directory listing exploit remediation. ###
What are two remediation methods for preventing directory listing exploits? <br>
- Disable Directory Indexing in Web Server Configuration: This is the most secure and direct method. By modifying the server configuration, you prevent the web server from automatically generating a file list when a default index file is missing. <br>
Apache: Remove Indexes from the Options directive in your httpd.conf or .htaccess file (e.g., Options -Indexes). <br>
Nginx: Ensure the autoindex directive is set to off within the server or location block. <br>
IIS: Use the IIS Manager to navigate to Directory Browsing and select Disable. <br>
- Add a Default Index File to Every Directory: As a fail-safe measure, place a default index file (e.g., index.html, index.php) in every folder within the web root. When a user accesses the directory, the web server will automatically serve this specific file instead of displaying the full directory contents. While effective as a "quick-fix," it is less robust than server-level configuration because new directories may inadvertently be created without such a file. <br>

# Challenge 3: Exploit open SMB Server Shares
*In this part, you want to discover if there are any unsecured shared directories located on an SMB server in the 10.5.5.0/24 network. You can use any of the tools you learned in earlier labs to find the drive shares available on the servers.* <br>

### Step 1: Scan for potential targets running SMB.
Use scanning tools to scan the 10.5.5.0/24 LAN for potential targets for SMB enumeration. <br>
using nmap 10.5.5.0/24 to scan for live hosts **10.5.5.14** in the subnet with open ports 139 and 445 which are associated with SMB services. <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_12_47_56" src="https://github.com/user-attachments/assets/29d46f48-2284-423b-9bf1-b9bb2281340c" />

Which host on the 10.5.5.0/24 network has open ports indicating it is likely running SMB services? <br>
Host 10.5.5.14 <br>
### Step 2: Determine which SMB directories are shared and can be accessed by anonymous users.
Use a tool to scan the device running SMB and locate the shares that can be accessed by anonymous users using the **enum4linux -S 10.5.5.14** command. <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_13_09_32" src="https://github.com/user-attachments/assets/fb85089d-08b3-424f-bd86-db4774fe1b51" />
<img width="1366" height="702" alt="Screenshot_2026-01-17_13_08_56" src="https://github.com/user-attachments/assets/3ce07edd-9196-4105-9fa0-95458ff3368d" />

What shares are listed on the SMB server? Which ones are accessible without a valid user login? <br>
using the command **smbmap -H 10.5.5.14** to see listed SMB shares and which are accessible without a valid user login <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_13_25_13" src="https://github.com/user-attachments/assets/35ff31cb-2acf-448e-92e0-13a9412ade71" />
Based on the smbmap output, the shares that are accessible without a valid user login (anonymous access) are: <br>
- workfiles - *READ ONLY* <br>
- print$ - *READ ONLY* <br>
The following shares are not accessible anonymously: <br>
- homes - *NO ACCESS* <br>
- IPC$ - *NO ACCESS* <br>
**workfiles and print$ are accessible without a valid user login** <br>

### Step 3: Investigate each shared directory to find the file.
Use the SMB-native client to access the drive shares on the SMB server. Use the dir, ls, cd, and other commands to find subdirectories and files. <br>
using the command **smbclient //10.5.5.14/print$ -N**. i tried **workfiles** but no files listed in it. <br>
Anonymous login successful <br>
dir <br>
cd OTHER <br>
ls <br>
- Locate the file with the Challenge 3 code. <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_14_07_39" src="https://github.com/user-attachments/assets/ecb90145-3f84-49f4-9772-a29c908a7b8a" />

- Download the file and open it locally. <br>
use **get sxij42.txt** to download the file to the local machine <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_14_17_34" src="https://github.com/user-attachments/assets/f317601c-c7cb-49da-9bc3-a65655cd7295" />
exit <br>
ls <br>
cat sxij42.txt <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_14_22_46" src="https://github.com/user-attachments/assets/60bccad6-18f9-4cc6-9f2c-31fbe99a8510" />
or open it with an editor **nano sxij42.txt** <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_14_23_15" src="https://github.com/user-attachments/assets/61767528-edee-4111-ba5b-6ecd86081319" />
- In which share is the file found? <br>
the file is found in print$ <br>
- What is the name of the file with Challenge 3 code? <br>
The file with challenge 3 code is **sxij42.txt** <br>
- Enter the code for Challenge 3 below. <br>
NWs39691 <br>

### Step 4: Research and propose SMB attack remediation.
What are two remediation methods for preventing SMB servers from being accessed? <br>
To prevent unauthorized access to SMB servers in 2026, use these two methods: <br>
- Block Port 445: Use firewalls to block all inbound and outbound traffic on TCP port 445. This prevents attackers from reaching the SMB service from the internet or unauthorized internal segments. <br>
- Disable Guest Logons: Configure Group Policy to disable "Insecure Guest Logons." This ensures that no user can access or even list available shares without providing valid, authenticated credentials. <br>

# Challenge 4: Analyze a PCAP File to Find Information.
*As part of your reconnaissance effort, your team captured traffic using Wireshark. The capture file, SA.pcap, is located in the Downloads subdirectory within the kali user home directory.*
<img width="1366" height="702" alt="Screenshot_2026-01-17_15_12_58" src="https://github.com/user-attachments/assets/af2431d4-8d10-4175-bda8-8a3b997dc279" />


### Step 1: Find and analyze the SA.pcap file.
Analyze the content of the PCAP file to determine the IP address of the target computer and the URL location of the file with the Challenge 4 code.
<img width="1366" height="702" alt="Screenshot_2026-01-17_15_31_51" src="https://github.com/user-attachments/assets/e9d0a7a2-6413-4eb6-ae50-761d1f34c852" />
By looking at the "Source" and "Destination" columns, we can see two primary IP addresses interacting: **10.5.5.1 and 10.5.5.11**. <br>
10.5.5.1 appears to be the client (the machine initiating requests). <br>
10.5.5.11 is the target computer (server). In packet No.4, the client sends a GET request to 10.5.5.11. in packet No.6, 10.5.5.11 responds with an HTTTP 200 OK, confirming it is hosting the web services. <br>

- What is the IP address of the target computer? <br>
10.5.5.11 <br>
- What directories on the target are revealed in the PCAP? <br>
<img width="1366" height="702" alt="Screenshot_2026-01-17_16_09_02" src="https://github.com/user-attachments/assets/8c340e0f-aefb-4611-b0ca-9d3aaca68c5b" />
http://10.5.5.11/database-offline.php, http://10.5.5.11/styles/global-styles.css, http://10.5.5.11/test/, http://10.5.5.11/data, http://10.5.5.11/webservices/rest/ws-user-account.php, http://10.5.5.11/includes, http://10.5.5.11/passwords, http://10.5.5.11/icons.text/gif, http://10.5.5.11/javascript/follow-mouse.js, http://10.5.5.11/webservices/soap/lib <br>

### Step 2: Use a web browser to display the contents of the directories on the target computer.
Use a web browser to investigate the URLs listed in the Wireshark output. Find the file with the code for Challenge 4. <br>








