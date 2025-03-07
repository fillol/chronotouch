# ChronoTouch

This Bash script is designed to automate changing the timestamps of files (modification date and time) within a folder, based on the date present in the filename or, preferably, in the EXIF metadata of images (if available). It is particularly useful for chronologically organizing photos and videos from various sources (camera, WhatsApp, screenshots, etc.).

## Main Features:

*   **Date Extraction:**
    *   **EXIF Priority:** For image files (`IMG`, `VID`), the script first attempts to extract the creation date from EXIF metadata using `exiftool`. If the EXIF date is available and valid, it is used.
    *   **Filename as Fallback:** If EXIF metadata is not available or does not contain a valid date, the script parses the filename to extract the date according to several predefined patterns (see "Supported Filename Formats" section).
*   **Timestamp Modification:** Uses the `touch -t` command to set the file timestamp to the extracted date.
*   **Recursive Handling:** The script can be executed on a specific folder, recursively processing all subfolders as well.
*   **Dry-run (Simulation) Mode:** The `-n` or `--dry-run` option allows running the script in simulation mode, displaying the changes that would be made without actually modifying timestamps.
*   **Supported File Formats:**
    *   **Images and Videos:** Prefixes `IMG`, `VID`, `PANO`.
    *   **Screenshots:** Prefix `Screenshot_`.
    *   **WhatsApp Images:** Recognition of the `IMG-YYYYMMDD-WA*` pattern (time is set to 00:00 for these files due to the lack of time information in typical WhatsApp filenames).

## Supported Filename Formats (for date extraction from filename):

*   `IMG-YYYYMMDD-HHMM*` (e.g., `IMG-20250306-1315.JPG` - Camera, Telegram, Instagram)
*   `IMG-YYYYMMDDWA*` (e.g., `IMG-20250306WA001.JPG` - WhatsApp images, time set to 00:00)
*   `VID-YYYYMMDD-HHMM*` (e.g., `VID-20250306-1315.MP4`)
*   `PANO-YYYYMMDD-HHMM*` (e.g., `PANO-20250306-1315.JPG`)
*   `Screenshot_YYYYMMDD_HHMMSS*` (e.g., `Screenshot_20250306_131530.png`)

## Prerequisites:

*   **Bash:** The script is written in Bash and requires a Unix-like environment (Linux, macOS, WSL).
*   **`exiftool` (optional, but recommended):** For extracting dates from EXIF metadata. Installable via package manager (e.g., `sudo apt install libimage-exiftool-perl` or `brew install exiftool`). If not installed, the script will still work by extracting dates only from filenames.
*   **`date`:** Standard system `date` command.

## How to Use:

1.  **Save the script:** Save the code to a file, for example, `chronotouch.sh`.
2.  **Make it executable:** `chmod +x chronotouch.sh`
3.  **Run in the desired folder:**

    *   **Current Folder:** `./chronotouch.sh` (answer `YES` when prompted).
    *   **Specific Folder (and subfolders):** `./chronotouch.sh path/to/folder`
    *   **Dry-run (Simulation) Mode in current folder:** `./chronotouch.sh -n` or `./chronotouch.sh --dry-run`
    *   **Dry-run (Simulation) Mode on specific folder:** `./chronotouch.sh -n path/to/folder`
    *   **Help:** `./chronotouch.sh -h` or `./chronotouch.sh --help`

## Important Notes:

*   **Initial Confirmation:** The first time you run the script without arguments in the current folder, confirmation is required by typing `YES` to proceed with timestamp modification.
*   **WhatsApp Images:** For WhatsApp images with the `IMG-DATA-WA*` pattern, the timestamp time will be set to `00:00` due to the lack of time information in the typical WhatsApp filename.
*   **Backup:** It is recommended to back up important files before running the script, especially the first time, for safety.

---

## GitHub Description (short and concise):

**`chronotouch.sh` - Bash Script to Organize Photo and Video Timestamps**

Chronologically organize your photos and videos! This Bash script automates file timestamp modification based on dates extracted from filenames or EXIF metadata (when available). Supports images from cameras, WhatsApp, screenshots, and more. Includes dry-run mode for simulation and recursive folder handling. Requires `exiftool` for full EXIF functionality (optional).

**[Link to GitHub repository (if applicable)]**  *(Add your GitHub repository link here if the script is hosted on GitHub)*
