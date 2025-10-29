# Folder Report Script 🌲📝

`folder-report` is a simple but powerful Bash script that generates a complete "snapshot" of a directory into a single text file.

It's designed for developers, students, and professionals who need to share a project's structure and all its code in one easy-to-read file. It creates a file that includes:

- A complete directory tree, just like the `tree` command (including hidden dotfiles).
- The full text content of every single file, printed one after another. **PDF text is automatically extracted!** 📄➡️💬

This is extremely useful for:

- Submitting code for a job application or school project.
- Sharing a project with a colleague for review.
- Pasting your *entire* project's context into an AI prompt.

---

## Features

- **All-in-One:** Creates one `_report.txt` file named after the folder you scan (e.g., scanning `my-project` creates `my-project_report.txt`).
- **Intelligent Ignoring:** Automatically skips common files and folders you don't want, such as `.git`, `__pycache__`, the secret `.env` file, and its own report files.
- **PDF Text Extraction:** Uses `pdftotext` to extract readable text from PDF files, making them searchable and useful in the report.
- **Easy to Install:** Can be installed as a system-wide command in seconds on Linux.
- **Customizable:** You can easily edit the script to add your own ignore patterns.

---

## Prerequisites (Linux)

This script relies on two common command-line tools:

1. **`tree`** — Used to generate the directory structure.  
2. **`pdftotext`** — Used to extract text from PDF files (part of the `poppler-utils` package).

Most Ubuntu/Debian-based systems already have `tree` installed. You might need to install `poppler-utils`.

Before installing the script, run this command to make sure you have the prerequisites:

```bash
sudo apt update && sudo apt install -y tree poppler-utils
```

---

## Installation (Linux)

This script is designed to be run as a command from your terminal.

### Step 1: Clone or Download

First, get the script onto your machine:

```bash
# Clone this repository
git clone https://github.com/YoussefMohamedym3/folder-report-script.git
cd folder-report-script
```

### Step 2: Install the Script

We’ll make the script executable and move it to a directory that is in your system’s `PATH`.

1. **Make it Executable:**

    ```bash
    chmod +x folder-report
    ```

2. **Move it to Your Local Bin:**

    This makes the command available from anywhere in your terminal.

    ```bash
    # This creates the ~/.local/bin directory if it doesn't exist
    mkdir -p ~/.local/bin

    # This moves the script into it
    mv folder-report ~/.local/bin/
    ```

3. **Refresh Your Terminal:**

    **Close your current terminal and open a new one.**  
    This allows your system to recognize the new command.

That’s it! The `folder-report` command is now ready to use.

---

## How to Use

Once installed, you can run the `folder-report` command from any directory.

#### To scan the directory you are currently in:

```bash
folder-report .
```

*(If your folder is named `my-project`, this will create `my-project_report.txt`)*

#### To scan a sub-folder:

```bash
folder-report ./src
```

*(This will create a file named `src_report.txt` in your current directory)*

#### To scan a completely different project:

```bash
folder-report /home/user/Documents/my-other-project
```

*(This will create a file named `my-other-project_report.txt`)*

---

## How to Customize Ignores

You can add your own files/folders to the ignore list.

1. Edit the script file:

    ```bash
    nano ~/.local/bin/folder-report
    ```

2. Add your patterns to the two places at the top of the file:

    - **`IGNORE_TREE_PATTERNS`:** Add your pattern here (separated by a `|`) to hide it from the `tree` view.  
    - **The `find` command:** Add a new `-o \( ... -prune \)` line to stop `find` from including its contents.

---

## License

This project is licensed under the **Apache 2.0 License**.
