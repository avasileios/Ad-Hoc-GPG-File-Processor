# Decryption Script: Ad Hoc GPG File Processor Va. Antonopoulos

## Overview

This document provides a complete and professional-level documentation of the custom Bash script developed to replace and improve a vital GPG-based decryption pipeline.

The new solution retains the exact logic and flow of the original scripts while eliminating dependencies on environment-based date variables. It is designed to be secure, robust, and easily executable manually (ad hoc), whenever needed.

---

## Goals

- Replace legacy batch-script system dependent on environment (`.bash_tede`, batch date variables)
- Keep **identical decryption behavior** as original
- Allow **manual, ad hoc execution**
- **Log all actions** to file and display key output live
- Ensure **safe, production-ready file handling** and error catching

---

## Technologies Used

- Language: **Bash shell script**
- Tools:
  - `gpg` for decryption
  - `unix2dos` for line-ending conversion
  - Core Unix utilities: `mkdir`, `cp`, `mv`, `rm`, `date`, `tee`

---

## Directory Layout

All operations are structured within the folder where the script resides:

```
project-folder/
├── decrypt_files.bsh              # Main executable script
├── input/                         # Encrypted .pgp files placed here
├── temp/                          # Temporary staging for processing
├── output/                        # Final decrypted files
├── processed/                     # Daily archive folder structure
│   └── YYYYMMDD/
│       ├── in/                    # Backup of input files
│       ├── in/orig/              # Backup of original input files
│       └── ot/                   # Archived decrypted output
└── logs/                          # Timestamped execution logs
```

---

## Script Workflow Summary

### 1. Initialization

- Current runtime date (`YYYYMMDD`) and timestamp are calculated.
- Logging is initialized: `logs/decrypt_YYYYMMDD_HHMMSS.log`

### 2. Directory Preparation

- Ensures all required directories (`input`, `output`, `temp`, `logs`, and date-based archive folders) exist.
- Creates them if missing.

### 3. Input Archival

- All `.pgp` files in `input/` are copied to:
  - `processed/YYYYMMDD/in/`
  - `processed/YYYYMMDD/in/orig/`

### 4. Staging for Decryption

- Input files are copied to the `temp/` folder for isolated processing.

### 5. File Decryption

- Each file is decrypted using `gpg` with passphrase `"testpass"`.
- Decrypted output is written using the original filename (without extension).
- Files are converted from Unix to DOS format using `unix2dos`.

### 6. Output Handling

- Decrypted files are copied to:
  - `output/`
  - `processed/YYYYMMDD/ot/`

### 7. Cleanup

- All processed files in `temp/` and `input/` are deleted.

---

## Logging

- All output is simultaneously:
  - Written to `logs/decrypt_YYYYMMDD_HHMMSS.log`
  - Echoed to the console for live monitoring
- Logs include:
  - Folder creation status
  - File processing steps
  - Success or error indicators with exit codes

---

## Security Considerations

- Passphrase  is hardcoded (as per legacy logic)
- GPG must have access to the correct private key
- No files are processed in-place; all processing is isolated to a temporary workspace

---

## Usage

1. Place encrypted `.pgp` files into the `input/` directory
2. Run the script:
   ```bash
   ./decrypt_files.bsh
   ```
3. Results will be:
   - Decrypted in `output/`
   - Archived in `processed/YYYYMMDD/`
   - Log stored in `logs/`

---

## Dependencies

- `bash`
- `gpg` (configured with necessary private key)
- `unix2dos` (installable via `dos2unix` package)

---

## Code Explanation

### Configuration Section

Initializes constants, working directories, and calculates current date/time values for runtime use. All paths are relative to the script's own directory.

### Logging Initialization

All output from the script is captured using `tee` to write both to screen and a timestamped log file under `logs/`.

### Function: `chk_abnd`

Central error-checking function. If the previous command failed (`$? != 0`), the script exits with an error. Otherwise, it prints a success message.

### Function: `prepare_dirs`

Ensures that the `input/`, `output/`, `temp/`, archive folders, and logs directory all exist. Creates any missing ones.

### Function: `archive_input`

Copies all input `.pgp` files to both `processed/YYYYMMDD/in/` and `in/orig/`, maintaining backup records of the encrypted input.

### Function: `stage_to_temp`

Copies files from `input/` to `temp/` where decryption happens. Prevents in-place modification of input files.

### Function: `decrypt_files`

Processes each file:

- Strips extension for output name
- Pipes the hardcoded passphrase to `gpg`
- Converts decrypted file to DOS format

### Function: `move_decrypted`

Uses `bash` extended globbing to:

- Copy all non-`.pgp` files to the output folder
- Move them to the archive (`ot/`) folder

### Function: `cleanup`

Deletes all files in `input/` and `temp/` to leave a clean state for the next run.

### Main Script Block

Runs all the above functions in a safe order:

1. Prepare directories
2. Archive inputs
3. Stage to temp
4. Decrypt files
5. Move outputs
6. Cleanup temp and input

Each step is logged in real time.

---

## Conclusion

This redesigned decryption pipeline ensures reliability, traceability, and safe execution while freeing the process from rigid batch scheduling. It is suitable for manual use by operations or system admins, and easily integrates into larger automation systems if needed later.

The system is modular, well-logged, and adheres to best practices for error handling and file safety.
