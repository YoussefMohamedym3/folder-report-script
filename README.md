# Folder Report Script 🌲📝

`folder-report` is a robust, fail-closed Bash utility that generates a comprehensive, **XML-formatted** snapshot of a codebase or directory structure. 

Optimized strictly for AI workflows and local inference engines (like vLLM, SGLang, or Ollama), it allows you to seamlessly ingest entire projects—including database schemas and PDF documentation—without bloating your context window with useless binary data, unescaped characters, or verbose code comments.

---

## 🚀 Core Features

* **Strict XML Architecture (NEW):** Abandons plaintext delimiters for a strict `<repository>`, `<directory_structure>`, and `<files>` XML hierarchy. This strongly grounds the context for LLM attention heads and prevents file contents from bleeding into system prompts.
* **Advanced Token Optimization (NEW):** Beyond ignoring junk folders, the script utilizes an integrated Perl pipeline to intelligently strip single-line and multi-line comments (`//`, `/* */`, `#`, ``) from supported source code files *before* injection, saving massive amounts of context space.
* **Fail-Closed Execution (NEW):** Runs under `set -euo pipefail` to ensure the script safely terminates on unbound variables or pipe failures rather than corrupting the output payload.
* **Safe HTML Escaping:** Automatically escapes special characters (`&`, `<`, `>`) inside source code, PDFs, and databases to ensure the final XML structure remains perfectly valid and parsable by upstream AI agents.
* **🧠 AI & Token Optimized:** * Intelligently ignores heavy build directories (`node_modules`, `target`, `dist`), IDE configs (`.idea`, `.vscode`), and common junk folders by default.
    * Automatically prunes massive lock files (`package-lock.json`, `Cargo.lock`) and server logs.
* **🛡️ Dual-Layer Binary Protection:**
    * **Extension Filtering:** Instantly skips known compiled binaries, media assets, and build files.
    * **Dynamic MIME-Type Inspection:** Acts as an enterprise-grade failsafe. It physically inspects file headers using the Linux `file` utility to block raw machine code from extension-less files, ensuring your report (and your terminal) is never corrupted.
* **📄 Smart Data Extraction:** * **PDFs:** Uses `pdftotext` to extract raw, searchable text from document assets (bypassing the comment stripper).
    * **SQLite Databases:** Dumps **schema only** by default to keep context lightweight, with an optional `--data` flag to inject full table rows.
* **⚙️ Graceful Fallbacks:** If optional dependencies like `tree` or `pdftotext` are missing on the host machine, the script will not crash; it will seamlessly fall back to native Bash alternatives or skip the unreadable files.
* **🎛️ Granular Context Control:** * **Include:** Rescue specific ignored items (like `dist`) using `--include` without dumping the rest of the junk.
    * **Exclude:** Completely hide specific directories/files from both the tree and the content dump using `--exclude`.
    * **Exclude Content:** Keep important structural folders in your tree map, but block their heavy code from inflating your context window using `--exclude-content`.

---

## 🛠️ Prerequisites (Linux)

While the script utilizes standard Bash utilities, it relies on a few common packages for advanced data extraction and regex processing:

```bash
# 'tree' for mapping, 'poppler-utils' for PDFs, 'sqlite3' for DBs, 'perl' for comment stripping
sudo apt update && sudo apt install -y tree poppler-utils sqlite3 file perl

```

---

## 💻 Installation

### Step 1: Clone the Repository

```bash
git clone [https://github.com/YoussefMohamedym3/folder-report-script.git](https://github.com/YoussefMohamedym3/folder-report-script.git)
cd folder-report-script

```

### Step 2: Install Globally

Make the script executable and **copy** it to your local binaries path (copying ensures the original file remains in your cloned repo so you can easily pull future updates):

```bash
chmod +x folder-report
mkdir -p ~/.local/bin
cp folder-report ~/.local/bin/

```

*(Note: You may need to restart your terminal or `source ~/.bashrc` if `~/.local/bin` was just created).*

---

## 📖 Usage Guide

Navigate to the directory you want to snapshot and run the command.

### 1. Standard Report (Recommended for AI Context)

Generates the XML directory tree, stripped code contents, and SQLite database **schemas** only. Ignores heavy folders like `node_modules` and `dist`.

```bash
folder-report .

```

*(Output is saved to `[folder-name]_report.xml`)*

### 2. Full Database Output

Includes the full output of standard mode, but also extracts and appends **all table rows** from any `.db` or `.sqlite` files.

```bash
folder-report . --data

```

### 3. Granular Control (Include specific ignored items)

If you need to inspect a typically ignored folder (like `dist`) or file (like `package-lock.json`), pass a comma-separated list to the `--include` flag.

```bash
folder-report . --include dist,package-lock.json

```

*(This extracts the items you explicitly asked for, while continuing to block other heavy folders like `node_modules`)*

### 4. Nuclear Override (Include ALL heavy directories)

Forces the script to traverse and extract *everything*, bypassing the default heavy directory ignores (it will still safely skip binary/media files).

```bash
folder-report . --include-all

```

### 5. Dynamic Exclusion (Ignore specific items completely)

If you have specific folders or files you want to omit from the report entirely (like `tests` or `dummy_data`), pass a comma-separated list to the `--exclude` flag.

```bash
folder-report . --exclude tests,dummy_data,secret_config.json

```

*(This prevents the specified items from being mapped in the tree or having their contents extracted).*

### 6. Tree-Only Mode (Exclude Content)

If you want a folder to show up in your directory tree so the AI knows it exists, but you don't want to dump its thousands of lines of code into the context window (e.g., test suites or migration files), use the `--exclude-content` flag.

```bash
folder-report . --exclude-content __tests__,migrations,scripts

```

*(These items will appear in the `<directory_structure>` but will be silently skipped in the `<files>` section).*

---

## 🗄️ SQLite Output Examples

**Standard Mode (Schema Only):**

```xml
<file path="./database.db">
CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT);
</file>

```

**Full Data Mode (`--data`):**

```xml
<file path="./database.db">
CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT);
INSERT INTO users VALUES(1,'Alice','alice@example.com');
</file>

```

---

## ⚙️ Customization

If you need to permanently tweak the core ignore lists for a specific tech stack, simply edit the configuration block at the top of the installed script:

```bash
nano ~/.local/bin/folder-report

```

Modify the core `add_dir_ignore` and `add_file_ignore` function calls to suit your permanent workflow.

---

## 📄 License

Distributed under the Apache 2.0 License.

