# 🔐 Decryption Script: Ad Hoc GPG File Processor  
*Author: Va. Antonopoulos*

![Bash](https://img.shields.io/badge/Script-Bash-blue?style=for-the-badge&logo=gnubash)
![GPG](https://img.shields.io/badge/Security-GPG-green?style=for-the-badge&logo=gnuprivacyguard)
![Logs](https://img.shields.io/badge/Logging-Enabled-orange?style=for-the-badge&logo=files)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)

---

## 📌 Overview
A custom **Bash script** designed to **replace and improve a legacy GPG-based decryption pipeline**.  
The script replicates the original logic but removes fragile environment dependencies, making it **secure, robust, and executable manually (ad hoc)** whenever needed.  

👉 For sysadmins:  
This ensures **safe decryption, proper archiving, and full traceability** with minimal dependencies.  

---

## 🎯 Goals
- 🛠️ Replace legacy batch-script system (environment `.bash_tede`)  
- 🔑 Maintain **identical decryption behavior**  
- ⚡ Enable **manual ad hoc execution**  
- 📜 Log all actions (file + console)  
- 🛡️ Ensure **safe file handling and error catching**  

---

## 💾 Technologies Used
- **Language**: Bash shell script  
- **Tools**:  
  - `gpg` → file decryption  
  - `unix2dos` → line-ending conversion  
  - Core Unix utilities: `mkdir`, `cp`, `mv`, `rm`, `date`, `tee`  

---

## 📂 Directory Layout
```
project-folder/
├── decrypt_files.bsh              # Main executable script
├── input/                         # Encrypted .pgp files
├── temp/                          # Staging area
├── output/                        # Final decrypted files
├── processed/                     # Daily archive
│   └── YYYYMMDD/
│       ├── in/                    # Backup of inputs
│       ├── in/orig/               # Original inputs
│       └── ot/                    # Archived decrypted output
└── logs/                          # Timestamped execution logs
```

---

## 🔄 Script Workflow

1. **Initialization** – Set date/time, start log file.  
2. **Directory Preparation** – Create missing folders.  
3. **Input Archival** – Copy `.pgp` files → archive (`in/`, `in/orig/`).  
4. **Staging** – Copy input files → `temp/`.  
5. **Decryption** – Run `gpg`, strip extension, convert with `unix2dos`.  
6. **Output Handling** – Move decrypted files → `output/` + archive (`ot/`).  
7. **Cleanup** – Delete processed files in `input/` + `temp/`.  

---

## 📝 Logging
- 📂 `logs/decrypt_YYYYMMDD_HHMMSS.log` created for each run  
- 🔄 Logs are written **to both console and file** with `tee`  
- ✅ Includes: folder creation, file handling steps, success/error messages  

---

## 🔒 Security Considerations
- Passphrase is **hardcoded** (legacy requirement)  
- Requires GPG private key access  
- Files never processed in-place → ensures data integrity  

---

## 🚀 Usage
1. Place `.pgp` files into the `input/` folder.  
2. Run the script:  
   ```bash
   ./decrypt_files.bsh
   ```
3. Results:  
   - 📂 Decrypted → `output/`  
   - 📦 Archived → `processed/YYYYMMDD/`  
   - 📝 Log → `logs/`  

---

## 📦 Dependencies
- `bash`  
- `gpg` (with correct private key configured)  
- `unix2dos` (from `dos2unix` package)  

---

## 🛠️ Code Highlights

### 🔧 Functions
- **`chk_abnd`** → Error handler, exits on failure.  
- **`prepare_dirs`** → Ensures required directories exist.  
- **`archive_input`** → Backups all `.pgp` inputs.  
- **`stage_to_temp`** → Copies files to temp for processing.  
- **`decrypt_files`** → Runs GPG decryption + converts to DOS format.  
- **`move_decrypted`** → Copies final files to output + archives.  
- **`cleanup`** → Removes processed temp/input files.  

### 🗂️ Main Script Flow
1. Prepare dirs  
2. Archive inputs  
3. Stage → temp  
4. Decrypt files  
5. Move outputs  
6. Cleanup  

---

## ✅ Conclusion
This redesigned **GPG decryption pipeline** is:  
- 🔐 Secure  
- ⚡ Reliable  
- 📜 Fully logged  
- 🛠️ Easy to run manually  

It’s ready for **system admins** handling sensitive files and can be integrated into **automation workflows** if needed.  
