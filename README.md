# Web-Application-pen-testing---SQL-INJECTION

# Web-Application-Pentesting

The primary objective of this authorized web application penetration testing laboratory is to cultivate practical competency in vulnerability assessment, exploitation analysis, and remediation reporting. Operating within a strictly controlled environment, participants gain hands-on experience in identifying and mitigating critical security flaws—including SQL injection, cross-site scripting (XSS), broken authentication mechanisms, and directory enumeration—while adhering to standardized security testing methodologies.

# The Lab Setup

Kali Linux (via VMware): Your operating system packed with security and hacking tools.

DVWA (Damn Vulnerable Web Application): Your safe, legal target practice website.

Burp Suite: Your tool to catch, view, and modify web traffic between your browser and the target app.


# STEP -1 
A) METASPLOIT 

Find your Metasploitable virtual machine's network IP address by running the ifconfig command in its terminal.
<img width="1920" height="991" alt="meta  Running  - Oracle VirtualBox 04_11_2026 09_23_16" src="https://github.com/user-attachments/assets/ff8bb512-793c-49a1-8ae7-faa3662265bf" />

B) DVWA

Open your web browser and go to the DVWA site by entering its IP address followed by the folder path in the address bar (192.168.1.104/DVWA/).
<img width="1920" height="1020" alt="Metasploitable2 - Linux - Personal - Microsoft​ Edge 04_11_2026 09_24_40" src="https://github.com/user-attachments/assets/bd2eab15-e81f-43c2-8bd8-17891c41ae90" />
<img width="1920" height="1020" alt="Metasploitable2 - Linux - Personal - Microsoft​ Edge 04_11_2026 09_24_21" src="https://github.com/user-attachments/assets/d3e61b95-3792-4cc1-8381-3cb1a8283658" />

# STEP - 2

Go to the main page of DVWA & then go to  security level section and select the (LOW) security option and click on SQL injection section.
<img width="1920" height="1020" alt="Metasploitable2 - Linux - Personal - Microsoft​ Edge 04_11_2026 09_24_48" src="https://github.com/user-attachments/assets/bc34fd3f-0383-4c67-a08e-59f9a828ba11" />
<img width="1920" height="1020" alt="Metasploitable2 - Linux - Personal - Microsoft​ Edge 04_11_2026 09_24_58" src="https://github.com/user-attachments/assets/9edb5a19-e9af-44c4-a42a-5bb964b00ca6" />

#  STEP - 3 

Enter the classic SQL injection trick (1' OR '1'='1) into the input field. Because this condition is always true, it forces the database to bypass security checks and hand over data or grant access.
<img width="1920" height="1020" alt="Metasploitable2 - Linux - Personal - Microsoft​ Edge 04_11_2026 09_49_45" src="https://github.com/user-attachments/assets/e4c3cf15-d831-4e94-b921-65c4ada0c8c3" />

Fetch databasese by typing ( 1' UNION SELECT 1, database()#)
<img width="1920" height="1020" alt="Metasploitable2 - Linux - Personal - Microsoft​ Edge 04_11_2026 09_52_18" src="https://github.com/user-attachments/assets/f3298534-dda3-4531-8511-d112356f606c" />

Now that we've identified the available databases (such as admin and dvwa), let's focus on dvwa. Knowing it contains a users table with username and password columns, enter this payload to extract all user credentials:

1' UNION SELECT user,password FROM users#
<img width="1920" height="1020" alt="Damn Vulnerable Web App (DVWA) v1 0 7 __ Vulnerability_ SQL Injection - Personal - Microsoft​ Edge 04_11_2026 09_54_12" src="https://github.com/user-attachments/assets/7f7d4d5d-cea4-4cd0-a718-8fa83ddd027c" />

Since the extracted passwords are encrypted hashes, we can use an online hash-cracking tool like CrackStation to look up and decode them back into plain text.
<img width="1920" height="1020" alt="Damn Vulnerable Web App (DVWA) v1 0 7 __ Vulnerability_ SQL Injection - Personal - Microsoft​ Edge 04_11_2026 09_55_53" src="https://github.com/user-attachments/assets/c8e81bd1-187b-4aec-8318-9f6b7e13a641" />

# STEP - 4 
Go to the security section again and click on medium and click on sumbit.

<img width="1920" height="1020" alt="CrackStation - Online Password Hash Cracking - MD5, SHA1, Linux, Rainbow Tables, etc  and 1 more page - Personal - Microsoft​ Edge 04_11_2026 09_57_22" src="https://github.com/user-attachments/assets/cde18bd2-5e3e-4107-a1c8-a8ab84bd358b" />

Since the webpage doesn't have a visible text box to type into, you'll need to use Burp Suite to catch and edit the web traffic directly before it reaches the server.
<img width="1920" height="1020" alt="Damn Vulnerable Web App (DVWA) v1 0 7 __ Vulnerability_ SQL Injection (Blind) - Personal - Microsoft​ Edge 04_11_2026 10_03_45" src="https://github.com/user-attachments/assets/e106b8c5-7c45-48f8-93be-479fc0d62539" />

Set up your browser to route its traffic through Burp Suite, then turn on Burp's intercept feature to capture and hold incoming and outgoing web requests.
<img width="1920" height="1020" alt="Burp Suite Community Edition v2026 9 3 - Temporary Project 04_11_2026 10_00_01" src="https://github.com/user-attachments/assets/46992ce6-b259-4b6c-aa05-22d8616d0ba5" />

# STEP - 5

Highlight the captured request in Burp Suite and send it to the Repeater tab (you can do this quickly by pressing Ctrl + E) so you can freely edit and resend it. Then, to test the database version, modify the parameter in your request with this payload: 1' UNION SELECT banner FROM v$version,null --.
<img width="1920" height="1020" alt="Damn Vulnerable Web App (DVWA) v1 0 7 __ Vulnerability_ SQL Injection (Blind) - Chromium 04_11_2026 10_08_27" src="https://github.com/user-attachments/assets/72ef3ac2-90c9-46d2-af0a-7d62f237c1c0" />

Inside Burp Suite, modify your request with the payload 1' UNION SELECT user,password FROM users# to extract all usernames and passwords from the users table.
<img width="1920" height="1020" alt="Burp Suite Community Edition v2026 9 3 - Temporary Project 04_11_2026 10_12_54" src="https://github.com/user-attachments/assets/e6d0b049-5f7a-4889-9938-072cc50453f3" />

# Task 2 :- Blind Sql Injection On DVWA.

Next, we'll look at Time-Based Blind SQL Injection. Since the page doesn't display direct data or errors, we trick the database into pausing for a set number of seconds (using a command like SLEEP). If the web page takes longer to load, it proves the vulnerability exists.

# STEP - 1 
Go to the dvwa and click on the blind sql injection.
<img width="1920" height="1020" alt="Damn Vulnerable Web App (DVWA) v1 0 7 __ Vulnerability_ SQL Injection (Blind) - Chromium 04_11_2026 10_17_07" src="https://github.com/user-attachments/assets/2c7b52a5-0f3b-40ba-8f63-f1d946ae46f0" />

When the page lacks a text box, you'll need to use Burp Suite to capture the traffic. Catch the request in Burp, copy it, and drop it into the Repeater tab by pressing Ctrl + E to easily modify and resend it.
<img width="1920" height="1020" alt="Burp Suite Community Edition v2026 9 3 - Temporary Project 04_11_2026 10_27_53" src="https://github.com/user-attachments/assets/7b7c1035-fda3-4d4c-914b-c4842ad9c924" />

Update the id parameter with a delay command (such as id=1 or sleep(5)) and hit send. If the server takes about 5 extra seconds to reply, it confirms the application is vulnerable.
<img width="1920" height="1020" alt="Burp Suite Community Edition v2026 9 3 - Temporary Project 04_11_2026 10_31_13" src="https://github.com/user-attachments/assets/692e39c1-5952-439c-89f9-4dbde3fa27b4" />

# How to Prevent SQL Injection?

Prepared Statements: Force the database to treat user input strictly as plain data, never as executable commands.

Input Validation & Sanitization: Filter and screen incoming data to ensure it matches what is expected before letting it anywhere near the database.

Least Privilege Principle: Limit database permissions so the application only has access to what it strictly needs, preventing total system compromise if breached.

Web Application Firewalls (WAF): Set up an automated digital security guard to monitor traffic and block malicious injection patterns before they reach your app.


# Skills Learned from Web Application Penetration Testing on DVWA:-

Architecture & Vulnerability Analysis: Gained foundational knowledge of web application structures and common flaws mapped to the OWASP Top 10.

SQL Injection Mastery: Acquired hands-on expertise in diverse exploitation vectors, including Union-based, Error-based, Boolean-based, and Time-based Blind SQLi.

Security Level Adaptation: Evaluated how varying DVWA security configurations impact attack surfaces and necessitate manual traffic manipulation via Burp Suite Repeater.

Defense & Mitigation: Mastered defense-in-depth principles, emphasizing secure coding practices and Web Application Firewall (WAF) deployment to neutralize injection risks.
