# ChronoTouch

This script is a command-line tool written in Bash to modify the timestamps (both "modified" and "accessed" times) of image and video files based on the date information extracted from their filenames or EXIF metadata.

It's designed to handle files with specific naming conventions commonly used by cameras, smartphones, and screenshot tools. The script prioritizes EXIF metadata for accuracy but falls back to filename parsing when EXIF data is not available or valid.

## Features

*   **Date Extraction from Filename:** Extracts date and time information from filenames matching predefined patterns for IMG, VID, PANO, Screenshot, and Schermata (Italian for Screenshot) prefixes.
*   **EXIF Metadata Extraction:** Optionally extracts the creation date from EXIF metadata (using `exiftool`) for potentially higher accuracy, especially for image and video files.
*   **Timestamp Modification:** Modifies the "modified" and "accessed" timestamps of the files using the extracted date.
*   **Dry-Run Mode:**  Simulates the timestamp modification process without actually changing any timestamps. Useful for testing and previewing changes.
*   **Verbose Mode:** Provides detailed output logging to the console, useful for debugging and understanding the script's actions.
*   **Interactive Mode:**  Prompts for user confirmation before modifying the timestamp of each file, adding an extra layer of safety.
*   **Recursive Folder Processing:** Can process all files within a specified folder and its subfolders recursively.
*   **Robust File Handling:**  Handles filenames with spaces and special characters correctly.
*   **Error Handling:** Includes basic error handling for `touch` command failures and warns about missing `exiftool`.

## Supported Filename Formats

The script recognizes the following filename formats for date extraction. It will try to extract date from EXIF metadata first for all recognized prefixes, and fallback to filename parsing if EXIF is not available or valid.

*   **IMG, VID, PANO Prefixes:**
    *   `IMGYYYYMMDDHHMM`
    *   `IMG-YYYYMMDD-HHMM`
    *   `IMG_YYYYMMDD_HHMM`
    *   `IMG-YYYYMMDDHHMM`
    *   `IMG_YYYYMMDDHHMM`
    *   `IMG-WA[0-9]+` (WhatsApp images, time generated from timestamp)
    *   `IMG_WA[0-9]+` (WhatsApp images, time generated from timestamp)
    *   `IMGWA[0-9]+` (WhatsApp images, time generated from timestamp)
    *   `IMG-YYYYMMDD-WA[0-9]+` (WhatsApp images, time generated from timestamp)
    *   `IMG_YYYYMMDD_WA[0-9]+` (WhatsApp images, time generated from timestamp)
    *   *(VID and PANO prefixes follow the same formats as IMG)*

*   **Screenshot and Schermata Prefixes:**
    *   `ScreenshotYYYYMMDDHHMMSS`
    *   `Screenshot-YYYYMMDD-HHMMSS`
    *   `Screenshot_YYYYMMDD_HHMMSS`
    *   `SchermataYYYYMMDDHHMMSS`
    *   `Schermata-YYYYMMDD-HHMMSS`
    *   `Schermata_YYYYMMDDHHMMSS`
    *   *(Both "Screenshot" and "Schermata" support underscore `_` or hyphen `-` as separators after the prefix and between date and time)*

**Note:**  For WhatsApp images (`IMG-WA...`, `IMG_WA...`, `IMGWA...` and similar for VID and PANO), the time is set to `00:00` as WhatsApp filenames in this format do not include time information.

## Usage

**Basic Usage:**

To run the script in the current directory, simply execute it without any arguments:

```bash
./chronotouch.sh
```

The script will prompt for confirmation before processing files in the current directory.

**Specifying a Folder:**

To process files in a specific folder and its subfolders, provide the folder path as an argument:

```bash
./chronotouch.sh /path/to/your/folder
```

**Options:**

*   `-n`, `--dry-run`:  **Dry-run mode (simulation).**  Runs the script without modifying any timestamps.  Use this mode first to preview the changes.

    ```bash
    ./chronotouch.sh -n
    ./chronotouch.sh --dry-run /path/to/folder
    ```

*   `-v`, `--verbose`:  **Verbose mode.** Enables detailed output, showing each step of the process, including date extraction attempts and results. Useful for debugging.

    ```bash
    ./chronotouch.sh -v
    ./chronotouch.sh --verbose /path/to/folder
    ```

*   `-i`, `--interactive`: **Interactive mode.**  Prompts for user confirmation before modifying the timestamp of each file.

    ```bash
    ./chronotouch.sh -i
    ./chronotouch.sh --interactive /path/to/folder
    ```

*   `-h`, `--help`: **Help message.** Displays the usage instructions and options.

    ```bash
    ./chronotouch.sh -h
    ./chronotouch.sh --help
    ```

**Examples:**

*   **Dry-run in the current directory:**
    ```bash
    ./chronotouch.sh -n
    ```

*   **Verbose mode in a specific folder:**
    ```bash
    ./chronotouch.sh -v /home/user/pictures
    ```

*   **Interactive mode in the current directory:**
    ```bash
    ./chronotouch.sh -i
    ```

*   **Interactive and verbose mode with dry-run in a folder:**
    ```bash
    ./chronotouch.sh -niv /home/user/photos
    ```

## Dependencies

*   **Bash:** The script is written in Bash and requires a Bash shell to run.
*   **exiftool (Optional but Recommended):**  For EXIF metadata extraction, the script uses `exiftool`.  `exiftool` is highly recommended for more accurate date extraction, especially for image and video files.

    **Installation of `exiftool` (Debian/Ubuntu):**

    ```bash
    sudo apt install libimage-exiftool-perl
    ```

    For other distributions or operating systems, refer to the `exiftool` documentation for installation instructions.

## Important Notes

*   **Run in Dry-Run Mode First:** It is **highly recommended** to run the script in dry-run mode (`-n` or `--dry-run`) first to preview the changes and ensure the script is behaving as expected before actually modifying any timestamps.
*   **Confirmation Required:** When running the script in the current directory without a folder argument, you will be prompted to type `YES` to confirm the operation. This is a safety measure to prevent accidental timestamp modifications in the wrong directory.
*   **No Date Found:** If the script cannot extract a valid date from either EXIF metadata or the filename for a particular file, it will output a "No date found..." message and will **not** modify the timestamp of that file.
*   **Error Handling:** The script includes basic error handling for the `touch` command. If the `touch` command fails to modify the timestamp (e.g., due to file permissions), an error message will be displayed.
*   **Backup Recommended:**  Before running the script on a large collection of files, it is always a good practice to **back up your files** as a precaution, especially if you are not using dry-run mode initially.
