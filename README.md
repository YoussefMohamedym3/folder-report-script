# Folder Report Script 🌲📝

`folder-report` is a simple but powerful Bash script that generates a
complete "snapshot" of a directory into a single text file.

It's designed for developers, students, researchers, and professionals
who need to share a project's structure and content in one readable
file. The generated report includes:

-   A complete directory tree (including hidden dotfiles)
-   The full text content of every file
-   **Smart Extraction:** Automatically converts PDFs and SQLite
    databases into readable text! 📄🗄️➡️💬
-   **Binary Safety:** Automatically skips content extraction for media (images, videos) and .pkl files while still listing them in the tree. 🚫📸

This is extremely useful for: - Pasting your *entire* project context
(including database schema) into an AI prompt - Submitting code for job
applications or school projects - Sharing a project with colleagues for
review

------------------------------------------------------------------------

## Features

-   **All-in-One Output:** Creates a single `_report.txt` file named
    after the scanned folder.
-   **Intelligent Ignoring:** Skips common junk files (like `.git`,
    `__pycache__`, and `.env`).
-   **PDF Extraction:** Uses `pdftotext` to extract searchable text from
    PDF files.
-   **SQLite Support:**
    -   **Default:** Dumps **schema only** (tables & columns) to keep
        the report small.
    -   **Optional:** Use `--data` to include all rows from all tables.
-   **Binary Safety:** Automatically detects and skips content dumping for images, videos, and .pkl files.

------------------------------------------------------------------------

## Prerequisites (Linux)

This script relies on three common tools:

1.  `tree`
2.  `poppler-utils` --- provides `pdftotext`
3.  `sqlite3`

Install them with:

``` bash
sudo apt update && sudo apt install -y tree poppler-utils sqlite3
```

------------------------------------------------------------------------

## Installation (Linux)

### Step 1: Clone or Download

``` bash
git clone https://github.com/YoussefMohamedym3/folder-report-script.git
cd folder-report-script
```

### Step 2: Install the Script

``` bash
chmod +x folder-report
mkdir -p ~/.local/bin
mv folder-report ~/.local/bin/
```

Restart your terminal afterward.

------------------------------------------------------------------------

## How to Use

### **1. Standard Report (Recommended)**

``` bash
folder-report .
```

### **2. Full Data Report**

``` bash
folder-report . --data
```

------------------------------------------------------------------------

## SQLite Output Examples

### **Standard Mode (Schema Only)**

    --- Content of: ./database.db ---
    >>> Extracting SQLite Schema Only (Use --data to see rows)...
    CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT);
    --- End of: ./database.db ---

### **With --data**

    --- Content of: ./database.db ---
    >>> Extracting FULL SQLite Schema AND Data...
    CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT);
    INSERT INTO users VALUES(1,'Alice','alice@example.com');
    --- End of: ./database.db ---

------------------------------------------------------------------------

## Customize Ignores

Edit the script:

``` bash
nano ~/.local/bin/folder-report
```

Modify ignore patterns in:

-   `IGNORE_TREE_PATTERNS`
-   The `find` command rules

------------------------------------------------------------------------

## License

Apache 2.0 License.
