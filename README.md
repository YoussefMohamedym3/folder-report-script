# Folder Report Script 🌲📝

`folder-report` is a simple but powerful Bash script that generates a
complete "snapshot" of a directory into a single text file.

It's designed for developers, students, researchers, and professionals
who need to share a project's structure and content in one readable
file. The generated report includes:

-   A complete directory tree (including hidden dotfiles)
-   The full text content of every code file
-   **Smart Extraction:** Automatically converts PDFs and SQLite
    databases into readable text! 📄🗄️➡️💬
-   **AI-Optimized Context:** Meticulously filters out non-code assets, massive lock files, and build artifacts to prevent token-bloat, making it perfect for feeding codebases into local LLMs (like vLLM or Ollama) or web-based AI assistants. 🤖🧠

This is extremely useful for: - Pasting your *entire* project context
(including database schema) into an AI prompt - Submitting code for job
applications or school projects - Sharing a project with colleagues for
review

------------------------------------------------------------------------

## Features

-   **All-in-One Output:** Creates a single `_report.txt` file named
    after the scanned folder.
-   **Intelligent Directory Ignoring:** Skips common junk folders, heavy build directories (`node_modules`, `target`, `build`, `dist`, `.next`), and IDE configs (`.vs`, `.idea`, `.vscode`).
-   **Token-Saving Filters:** Automatically prunes massive dependency lock files (`package-lock.json`, `Cargo.lock`, `go.sum`), heavy datasets (`.csv`), and server logs (`.log`).
-   **PDF Extraction:** Uses `pdftotext` to extract searchable text from PDF files.
-   **SQLite Support:**
    -   **Default:** Dumps **schema only** (tables & columns) to keep the report small.
    -   **Optional:** Use `--data` to include all rows from all tables.
-   **Binary Safety:** Automatically detects and skips content dumping for media (images/video/audio), Microsoft Office files, and compiled binaries (C++ `.obj`/`.tlog`, Java `.jar`, Rust, etc.) while still listing them in the tree. 🚫📸

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
