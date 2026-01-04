# Writeup: TryHackMe - Phishing Analysis Fundamentals

**Student Name:** Tengku Eikmal Aqil Bin Tengku Sifzizul  
**Date:** 4th January  
**Platform:** TryHackMe  
**Room:** Phishing Analysis Fundamentals  
**Category:** Phishing / Email Forensics  

## 1. Executive Summary
This report details the analysis of a suspicious email provided in the "Phishing Analysis Fundamentals" laboratory. The objective was to dissect the email components—headers, body, and attachments—to identify indicators of compromise (IOCs) and confirm the malicious nature of the message. The analysis confirmed the email was a phishing attempt masquerading as a "Home Depot" order confirmation.

## 2. Methodology
The investigation followed a standard email forensics process:
1.  **Header Analysis:** Examining the "From", "Return-Path", and "Received" fields for inconsistencies.
2.  **Body Inspection:** Checking for urgency, typos, and mismatched branding.
3.  **URL & Attachment Analysis:** Defanging malicious links and reconstructing hidden files.

## 3. Investigation Walkthrough

### Task 1: Header Analysis
I examined the email headers to establish the true origin of the message.
* **Findings:**
    * **Subject Line:** The email claimed to be an order update: *Order Placed : Your Order ID OD2321657089291 Placed Successfully*.
    * **Sender Identity:** The email claimed to be from "Home Depot", but the `From` address was `support@teckbe.com`.
    * **Return-Path:** The return path matched the suspicious sender (`support@teckbe.com`) rather than an official Home Depot domain.

### Task 2: Analyzing the Body & URLs
The email body contained a "Click Here" link urging the user to view their order.
* **Link Analysis:** I extracted the URL and "defanged" it to prevent accidental execution.
* **Malicious URL:** `hxxp[://]t[.]teckbe[.]com/p/?j3=EOo...`
* **Observation:** The domain `teckbe.com` is unrelated to Home Depot, a clear indicator of credential harvesting or malware delivery.

### Task 3: Attachment Reconstruction
The lab provided a text file (`email2.txt`) containing raw email data, including a large block of Base64 encoded text.
* **Technique:** I used **CyberChef** to decode the Base64 string.
* **Process:** `Input (Base64 text) -> From Base64 -> Output (PDF file)`.
* **Result:** The decoded data revealed a PDF file. Upon opening the reconstructed PDF, I found the hidden flag for the exercise.

## 4. Conclusion
The email is a confirmed phishing attempt.
* **Impersonation:** It falsely claims to be from a major retailer (Home Depot).
* **Inconsistency:** The sending domain (`teckbe.com`) does not match the claimed identity.
* **Malicious Intent:** The email attempts to lure the user into clicking a link hosted on a third-party domain under the guise of an order confirmation.

### Key Tools Used
* **CyberChef:** For decoding Base64 attachments.
* **Text Editor:** For inspecting raw email headers.
* **Terminal:** For manipulating file types (converting text dumps to PDF).
