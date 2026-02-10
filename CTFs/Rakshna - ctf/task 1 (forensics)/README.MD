# 🕵️ RAKSHNA Recruitment CTF – Forensics Write-up

## 📌 Task 1: The Ghost in the Header

### 🧩 Problem Statement
One of our senior members tried to send over a top-secret recruitment image, but the file got intercepted and *sanitized* by a rival society. All we have is this weird blob of data. We know it was an image, but now nothing will open it.

**Objective:** Recover the hidden secret from the corrupted file.  
**Flag Format:** `RAKSHNA{...}`

---

## 📁 Files Provided
- `task1` (corrupted file with no extension)

---

## 🧠 Initial Analysis
I started by opening the file in **Notepad** to understand what kind of data it contained.  
Most of it looked like random dump data, but I noticed a familiar string: IHDR

From basic knowledge, I knew that `IHDR` is a critical chunk used in **PNG images**.  
This hinted that the file was originally a PNG, but its **header was corrupted or removed**, which explained why it wouldn’t open normally.

📸 **Screenshot:** Opening the file in Notepad showing `IHDR`

---

## 🔎 Research & Tool Used
Since the file could not be opened using normal image viewers, I researched about:
- image file headers
- file corruption
- hex editing

This led me to **HxD**, a hex editor that allows direct modification of file bytes.

---

## 🧪 Hex Analysis
I opened the file in **HxD** and navigated to the beginning of the file (offset `0x00`).

A valid PNG file must start with the following **8-byte signature**:

**89 50 4E 47 0D 0A 1A 0A**

However, the provided file did not contain this correct header, confirming that the PNG signature was altered.

📸 **Screenshot:** HxD showing incorrect header bytes

---

## 🛠️ Fixing the PNG Header
To fix the file:
1. I replaced the **first 8 bytes** with the correct PNG header

2. Saved the file
3. Renamed it from `task1` to `task1.png`

📸 **Screenshot:** HxD after fixing the PNG header

---

## ❌ Errors & Learning Moments
During the process, I made a few mistakes:
- I initially edited the **wrong hex values**, which made the image unreadable
- I forgot to rename the file to `.png` at one point, so it still wouldn’t open
- I had to revert changes using the `.bak` file and redo the steps carefully

These mistakes helped me understand how:
- even a single incorrect byte can corrupt a file
- file extensions matter after header repair
- backups are extremely useful in forensic work

📸 **Screenshots:**
- Broken image after wrong hex edits  
- Error due to missing `.png` extension  

---

## 🎉 Final Result
After correcting the header and opening the file successfully, the image displayed a message with the flag written in plain text.

📸 **Screenshot:** Opened image displaying the flag

---

## 🚩 Flag
RAKSHNA{h3xdump_h3r0_2026}


---

## 📚 What I Learned
- Basics of **file headers and signatures**
- PNG file structure and the role of `IHDR`
- Practical usage of **HxD hex editor**
- Importance of precision in digital forensics
- Learning tools while solving a problem is part of the CTF process

---

✅ *This task helped me gain hands-on experience with file forensics and hex-level analysis, making it a great learning experience as a beginner in cybersecurity.*

