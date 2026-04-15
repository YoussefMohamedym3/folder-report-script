# Folder Report Script 🌲📝

`folder-report` is a robust Bash utility that generates a comprehensive, single-file text snapshot of a codebase or directory structure. 

Originally designed for developers, researchers, and students, this tool is **optimized for AI workflows**. It allows you to seamlessly ingest entire projects—including database schemas and PDF documentation—into local LLMs (like vLLM or Ollama) or web-based AI assistants without bloating your context window with useless binary data.

---

## 🚀 Core Features

* **All-in-One Context:** Compiles a complete directory tree and the full text of all readable files into a single, easily shareable `_report.txt` file.
* **🧠 AI & Token Optimized:** * Intelligently ignores heavy build directories (`node_modules`, `target`, `dist`), IDE configs (`.idea`, `.vscode`), and common junk folders.
    * Automatically prunes massive lock files (`package-lock.json`, `Cargo.lock`) and server logs to preserve LLM token limits.
* **🛡️ Dual-Layer Binary Protection (NEW):**
    * **Extension Filtering:** Instantly skips known compiled binaries, media assets, and build files.
    * **Dynamic MIME-Type Inspection:** Acts as an enterprise-grade failsafe. It physically inspects file headers using the Linux `file` utility to block raw machine code from extension-less files, ensuring your report (and your terminal) is never corrupted.
* **📂 Safe Traversal (NEW):** Utilizes null-terminated string processing to safely handle complex filenames containing spaces, special characters, or emojis without breaking the script.
* **📄 Smart Data Extraction:** * **PDFs:** Uses `pdftotext` to extract raw, searchable text from document assets.
    * **SQLite Databases:** Dumps **schema only** by default to keep context lightweight, with an optional `--data` flag to inject full table rows.
* **⚙️ Graceful Fallbacks (NEW):** If optional dependencies like `tree` or `pdftotext` are missing on the host machine, the script will not crash; it will seamlessly fall back to native Bash alternatives or skip the unreadable files.

---

## 🛠️ Prerequisites (Linux)

While the script utilizes standard Bash utilities, it relies on a few common packages for advanced data extraction:

```bash
# 'tree' for mapping, 'poppler-utils' for PDF text, 'sqlite3' for database schemas
sudo apt update && sudo apt install -y tree poppler-utils sqlite3 file
```

---

## 💻 Installation

### Step 1: Clone the Repository
```bash
git clone [https://github.com/YoussefMohamedym3/folder-report-script.git](https://github.com/YoussefMohamedym3/folder-report-script.git)
cd folder-report-script
```

### Step 2: Install Globally
Make the script executable and move it to your local binaries path:
```bash
chmod +x folder-report
mkdir -p ~/.local/bin
mv folder-report ~/.local/bin/
```
*(Note: You may need to restart your terminal or `source ~/.bashrc` if `~/.local/bin` was just created).*

---

## 📖 Usage Guide

Navigate to the directory you want to snapshot and run the command.

### 1. Standard Report (Recommended for AI Context)
Generates the directory tree, all code contents, and SQLite database **schemas** only.
```bash
folder-report .
```

### 2. Full Data Report
Includes the full output of standard mode, but also extracts and appends **all table rows** from any `.db` or `.sqlite` files.
```bash
folder-report . --data
```

---

## 🗄️ SQLite Output Examples

**Standard Mode (Schema Only):**
```sql
--- Content of: ./database.db ---
>>> Extracting SQLite Schema Only (Use --data to see rows)...
CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT);
--- End of: ./database.db ---
```

**Full Data Mode (`--data`):**
```sql
--- Content of: ./database.db ---
>>> Extracting FULL SQLite Schema AND Data...
CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT);
INSERT INTO users VALUES(1,'Alice','alice@example.com');
--- End of: ./database.db ---
```

---

## ⚙️ Customization

If you need to tweak the ignore lists for a specific tech stack, simply edit the configuration block at the top of the script:

```bash
nano ~/.local/bin/folder-report
```
Modify the `IGNORE_TREE_PATTERNS` or append new extensions to the `Regex` check within the `find` loop.

---

## 📄 License

Distributed under the Apache 2.0 License.
