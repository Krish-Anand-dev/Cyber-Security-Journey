RAKSHNA Recruitment CTF Write-up

# RAKSHNA CTF Write-Up

**Date Completed:** February 10, 2026  
**Difficulty:** Beginner–Intermediate  
**Format:** Multi-domain CTF (Forensics, Crypto, OSINT, Web)  
**Context:** Society recruitment challenge  
**Status:** Completed (All tasks solved)

---

## Overall Summary

This was my first Capture The Flag (CTF) experience, and it exposed me to multiple cybersecurity domains including OSINT, Forensics, Cryptography, and Web exploitation. Each challenge required a different way of thinking — from investigative searching to byte-level file repair and layered decryption.

Some tasks were straightforward, while others involved research, experimentation, and learning new tools during the process. I encountered multiple mistakes, especially in the cryptography and forensics challenges, but those mistakes became the most valuable learning points.

By the end of the CTF, I became more comfortable analyzing files, inspecting web source code, using tools like hex editors and CyberChef, and approaching problems methodically instead of randomly guessing.

This experience significantly improved my practical understanding of how real-world cybersecurity challenges are structured.

---

# Task 1: The Trail of Breadcrumbs (OSINT)

**Category:** OSINT  
**Flag Format:** RAKSHNA{...}

## Challenge Description

The challenge hinted that the society’s social media platforms were being used to post devotional or wisdom-related content. The objective was to trace these “digital breadcrumbs” and uncover the hidden flag.

## Approach

Based on the hint, I began investigating:

- Official website  
- Instagram  
- LinkedIn  
- Other connected social platforms  

Initially, I could not find anything suspicious within posts or captions on the main accounts.

While reviewing Instagram more carefully, I discovered a linked account named **"Daily Wisdoms"**. One of its posts contained an external link in the caption.

Opening the link redirected to a Paste page where the flag was written in plain text.

## Flag

RAKSHNA{f0ll0w_th3_d1g1t4l_f00tpr1nt_b4ck_t0_m3}


## Key Takeaways

- OSINT relies heavily on observation and attention to detail  
- External links and secondary accounts are important  
- Small hints in descriptions can guide the entire solution  

---

# Task 2: The Ghost in the Header (Forensics)

**Category:** Forensics  
**Flag Format:** RAKSHNA{...}

## Challenge Description

An image file had been intercepted and “sanitized,” making it impossible to open. The goal was to recover the original image and extract the hidden flag.

## Initial Analysis

I opened the file using Notepad to inspect its contents. Most of the data appeared random, but I noticed the string:

IHDR

Since `IHDR` is a known chunk in PNG files, this indicated that the file was originally a PNG image but its header was likely corrupted or removed.

## Research and Tools

After researching file headers and image signatures, I learned about hex editors and decided to use **HxD** to analyze the file at the byte level.

## Fixing the File

A valid PNG file begins with the following 8-byte signature:

89 50 4E 47 0D 0A 1A 0A

Steps followed:

1. Opened the file in HxD  
2. Replaced the first 8 bytes with the correct PNG signature  
3. Saved the file  
4. Renamed the file extension to `.png`  

## Errors Encountered

- Initially edited incorrect bytes, corrupting the file  
- Forgot to rename the file to `.png`  
- Had to revert changes using a backup file  

These mistakes highlighted how sensitive file headers are and how a single incorrect byte can make a file unreadable.

## Flag

RAKSHNA{h3xdump_h3r0_2026}


## Key Takeaways

- Understanding file signatures is essential in digital forensics  
- Hex editors allow precise control over file data  
- Accuracy is critical when modifying binary files  

---

# Task 3: 3-STEP VERIFICATION (Crypto)

**Category:** Cryptography  
**Flag Format:** RAKSHNA{...}

## Challenge Description

An intercepted message was protected with multiple layers of encoding. The description hinted that the final layer was a Vigenère cipher using the society’s name as the key.

Encrypted fragment:

IU5IJsIBKndoGJxaZjTmZWQfJ3MgK2o0sF8gQEyswQ==

## Initial Approach

Since I was unfamiliar with layered cryptographic challenges, I researched tools and discovered **CyberChef**.

My early attempts included:

- Decoding Base64 first, then applying Vigenère  
- Inserting additional text decoding steps  
- Forgetting to properly interpret output as UTF-8  

These attempts did not produce meaningful results.

## Correct Method

After carefully re-reading the challenge and experimenting systematically, I determined the correct decoding sequence:

1. Apply Vigenère decryption using key `rakshna`  
2. Decode the result from Base64  
3. Identify the final output as ROT13 and decode it  

Applying the steps in the correct order revealed the flag.

## Flag

RAKSHNA{crypt0_1s_th3_w4y_0nly}


## Key Takeaways

- The order of operations is critical in cryptography  
- CyberChef is highly effective for multi-layer decoding  
- Recognizing encoding patterns like ROT13 is useful  

---

# Task 4: The Administrator's Oversight (Web)

**Category:** Web  
**Flag Format:** RAKSHNA{...}

## Challenge Description

A prototype login portal contained developer oversights. The flag was split into three fragments hidden in:

- Styles  
- Elements  
- Script  

The goal was to reconstruct the full flag using browser diagnostic tools.

## Approach

I opened the website and used the browser’s **Inspect Element** feature.

Following the hints:

- One fragment was found inside a commented section in the HTML  
- Another fragment was located within the body under a function  
- The final fragment was embedded inside the script section  

Combining the three parts reconstructed the complete flag.

## Flag

RAKSHNA{alw4ys_ch3ck_th3_s0urc3_cod3}


## Key Takeaways

- Always inspect page source when dealing with web challenges  
- Developers often leave sensitive information in comments or scripts  
- Browser developer tools are powerful and essential  

---

## Final Reflection

This CTF strengthened my understanding of multiple cybersecurity domains and improved my problem-solving approach. It also taught me that mistakes and experimentation are part of the learning process.

Completing all tasks gave me confidence and motivated me to continue practicing and documenting future CTF challenges.
