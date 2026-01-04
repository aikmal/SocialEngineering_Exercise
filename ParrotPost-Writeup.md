# Writeup: TryHackMe - ParrotPost (Phishing Analysis)

**Student Name:** Tengku Eikmal Aqil Bin Tengku Sifzizul  
**Date:** 4th January
**Platform:** TryHackMe  
**Room:** ParrotPost  
**Category:** Social Engineering / Phishing Analysis  

## 1. Executive Summary
In this lab, I investigated a suspicious email to determine if it was a legitimate request or a phishing attempt. The investigation involved analyzing email headers for spoofing, examining malicious attachments, and de-obfuscating JavaScript code designed to steal user credentials.

## 2. Methodology & Tools
* **Static Analysis:** Inspecting raw email headers and source code without executing the file.
* **Code De-obfuscation:** Using manual analysis to decode Base64 and unescape HTML strings.
* **Tools Used:** * Text Editor (Sublime Text/Notepad)
    * CyberChef (for decoding)
    * Browser Developer Tools

## 3. Walkthrough & Findings

### Step 1: Email Header Analysis
I began by viewing the raw `.eml` file to inspect the headers.
* **Sender:** The email claimed to be from `ParrotPost`.
* **Red Flag Identified:** I checked the `Reply-To` address and noticed a discrepancy. While the legitimate domain is `parrotpost.thm`, the `Reply-To` address was set to a typosquatted domain: `no-reply@postparr0t.thm` (notice the '0' instead of 'o').
* **Conclusion:** This indicates that any reply sent by the user would go to the attacker, not the legitimate service.

### Step 2: Attachment Analysis
The email contained an attachment named `ParrotPostACTIONREQUIRED.htm`.
* **Suspicion:** Phishers often use `.htm` or `.html` attachments to bypass email gateways and host the phishing page locally on the victim's computer.
* **Source Code Inspection:** I opened the file in a text editor. Instead of standard HTML, I found a block of obfuscated JavaScript.

### Step 3: Code De-obfuscation
The malicious code used a common obfuscation technique to hide its true intent.
* **The Code:**
    ```javascript
    var b64 = "[LONG_BASE64_STRING]";
    document.write(unescape(atob(b64)));
    ```
* **Analysis:**
    * `atob(b64)`: Decodes the Base64 string into binary data.
    * `unescape()`: Converts the binary data into a readable string.
    * `document.write()`: Writes the malicious HTML content onto the page dynamically.
* **Decoding:** By decoding the Base64 string, I revealed the hidden HTML, which was a fake login page designed to look exactly like the ParrotPost login portal.

### Step 4: Credential Harvesting
I examined the decoded HTML forms to see where the data was being sent.
* **Action:** I identified the `action` attribute in the form tag.
* **Finding:** When a user enters their credentials, they are sent to a malicious PHP script (e.g., `/cred-capture.php`) controlled by the attacker.
* **Testing:** I submitted fake credentials to the form. The server responded with a success message, confirming the credential theft mechanism.

## 4. Conclusion
The "ParrotPost" email is a confirmed phishing attack. It utilizes **Typosquatting** in the `Reply-To` header to mislead victims and **HTML/JavaScript Obfuscation** to bypass automated scanners.

### Key Lessons Learned:
1.  Always verify the `Reply-To` header, not just the `From` address.
2.  Be wary of `.htm` attachments that claim to require urgent action.
3.  Obfuscated JavaScript (like `document.write`) in an email attachment is a primary indicator of malicious intent.
