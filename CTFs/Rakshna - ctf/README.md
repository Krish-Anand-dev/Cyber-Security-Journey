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

## **Flag**

**RAKSHNA{f0ll0w_th3_d1g1t4l_f00tpr1nt_b4ck_t0_m3}**

<img width="1896" height="856" alt="Screenshot 2026-02-10 204343" src="https://github.com/user-attachments/assets/6e1088a1-859a-4799-894b-2d78e187d301" />

## Key Takeaways

- OSINT relies heavily on observation and attention to detail  
- External links and secondary accounts are important  
- Small hints in descriptions can guide the entire solution  

---

# Task 2: The Ghost in the Header (Forensics)

**Category:** Forensics  
**Flag Format:** RAKSHNA{...}

<img width="1416" height="715" alt="Screenshot 2026-02-10 205516" src="https://github.com/user-attachments/assets/b83710fb-4014-4772-8a95-3c48c0de84c6" />

<img width="1418" height="724" alt="Screenshot 2026-02-10 205621" src="https://github.com/user-attachments/assets/dc21e3d8-5a16-478c-9fde-4e1f1dcbb9d1" />

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

<img width="901" height="312" alt="Screenshot 2026-02-10 210809" src="https://github.com/user-attachments/assets/e676ea03-0575-4d33-9074-634acfd25458" />


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

<img width="1420" height="730" alt="Screenshot 2026-02-10 211454" src="https://github.com/user-attachments/assets/90adf95d-1a36-44b0-9985-4781dd3b1c28" />

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

<img width="1916" height="871" alt="Screenshot 2026-02-10 213021" src="https://github.com/user-attachments/assets/df2a9a8a-a8ac-4b42-a5dd-f4e9dc20990b" />
<img width="1915" height="844" alt="Screenshot 2026-02-10 214024" src="https://github.com/user-attachments/assets/67e7f043-a8ce-4248-9b34-c32e30fabb66" />

## Correct Method

After carefully re-reading the challenge and experimenting systematically, I determined the correct decoding sequence:

1. Apply Vigenère decryption using key `rakshna`  
2. Decode the result from Base64  
3. Identify the final output as ROT13 and decode it  

Applying the steps in the correct order revealed the flag.
<img width="1541" height="833" alt="Screenshot 2026-02-10 215616" src="https://github.com/user-attachments/assets/0e92ccc5-278a-4841-be69-ee4e6302022f" />

## Flag

RAKSHNA{crypt0_1s_th3_w4y_0nly}

<img width="1890" height="869" alt="Screenshot 2026-02-10 215735" src="https://github.com/user-attachments/assets/765f7cf3-ad2e-4607-8a00-0d0677f77bc7" />

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

<img width="1892" height="884" alt="Screenshot 2026-02-10 220305" src="https://github.com/user-attachments/assets/9d0686d5-65b5-4717-8425-9c5d6730a088" />

<img width="1918" height="877" alt="Screenshot 2026-02-10 220448" src="https://github.com/user-attachments/assets/7dc9cbf6-aa72-4475-87ab-4b164c10eec4" />

<img width="1326" height="626" alt="Screenshot 2026-02-10 220602" src="https://github.com/user-attachments/assets/fc5c7814-cbf9-493a-9c9e-15b3ed12ea42" />

## Key Takeaways

- Always inspect page source when dealing with web challenges  
- Developers often leave sensitive information in comments or scripts  
- Browser developer tools are powerful and essential  

---

## Final Reflection

This CTF strengthened my understanding of multiple cybersecurity domains and improved my problem-solving approach. It also taught me that mistakes and experimentation are part of the learning process.

Completing all tasks gave me confidence and motivated me to continue practicing and documenting future CTF challenges.
