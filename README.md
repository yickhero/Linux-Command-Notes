# Linux Command Notes


## File & Directory Operations

* **`ls`** — Lists files and directories.
* `ls -a` : Shows all files, including hidden ones (files starting with `.`).


* **`cat`** — Prints the contents of a file to the terminal.
* `cat ./-` : Reads a file literally named `-` (using `./` prevents the command from freezing by stopping it from reading standard input).
* `cat ./"--spaces in filename"` : Uses quotes to read a file with spaces in its name.


* **`mv`** — Moves or renames files and directories.
* `mv secret.txt /tmp/secrets` : Moves the file to the `/tmp/secrets` directory.
* `mv oldname.txt newname.txt` : Renames the file in the current directory.


* **`file`** — Determines file types (e.g., ASCII text, executable, data).
* `file ./*` : Safely checks the file type of everything in the current directory, even if filenames start with special characters.


* **`find`** — Searches for files within a directory hierarchy based on specific criteria.
* `find -type f -size 1033c ! -executable` : Finds regular human-readable files exactly 1033 bytes in size that are *not* executable.


* **`./`** — Executes a file in the current directory.
* `./bandit.do cat /etc/bandit_pass/bandit20` : Executes a setuid binary which temporarily grants the permissions of another user (`bandit20`) to run the `cat` command.



## Text & Data Processing

* **`grep`** — Searches for a specific pattern or word and prints the matching lines.
* `grep "millionth" data.txt` : Finds and prints the line containing the word "millionth".


* **`sort`** — Sorts lines of text files alphabetically or numerically.
* `sort data.txt | uniq -u` : Sorts the file first (required for `uniq` to work properly), then pipes it to `uniq` to isolate non-duplicated lines.


* **`uniq`** — Filters or reports repeated lines. *Note: Duplicate lines must be adjacent to be detected.*
* **`strings`** — Extracts and prints human-readable text characters from binary or compiled files.
* `strings data.bin` : Useful for finding hidden passwords or strings inside machine code.


* **`tr`** — Translates or deletes characters (e.g., case conversion or Caesar ciphers).
* `echo "hello world" | tr 'a-z' 'A-Z'` : Converts all lowercase letters to uppercase.
* `tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt` : Applies a ROT13 cipher (shifts letters 13 spaces) by feeding the file into the command.


* **`diff`** — Compares two files line by line and isolates the differences.
* `diff password.old password.new` : Shows exactly what changed between the two versions.



## Encoding & Hex Dumping

* **`base64`** — Encodes or decodes data into Base64 format (64 safe ASCII characters).
* `base64 -d data.txt` : Decodes a Base64-encoded file back to its original state.


* **`xxd`** — Creates a hex dump of a file (displays raw binary as hex values) or does the reverse.
* `xxd -r data.txt > data.bin` : Reverses a hex dump back into a raw binary file.



## Archiving & Compression

* **`gzip` / `bzip2**` — File compression utilities.
* `mv data.bin data.gz` → `gzip -d data.gz` : Renames the file to the correct extension, then decompresses it.


* **`tar`** — Tape Archive. Groups multiple files into a single `.tar` file (similar to a `.zip`).
* `tar -cf new_archive.tar folder_to_pack/` : Packs a folder into a new `.tar` archive.
* `tar -xf archive.tar` : Unpacks/extracts the contents of a `.tar` file.



## Networking & Connectivity

* **`ssh`** — Secure Shell. Connects securely to a remote machine.
* `ssh -p 2220 bandit22@bandit.labs.overthewire.org` : Connects to the host on port 2220.
* `ssh -p 2220 bandit18@... "cat ./readme"` : Connects and immediately executes a command without dropping into an interactive shell.


* **`nc` (Netcat)** — Reads and writes data across network connections.
* `nc localhost 30000` : Connects directly to port 30000 on the local machine.


* **`ncat`** — An advanced version of Netcat with additional features like SSL/TLS support.
* `ncat --ssl localhost 30001` : Connects to port 30001 using SSL encryption.


* **`nmap`** — Network mapper used to discover open ports and services.
* `nmap -p 31000-32000 localhost` : Scans the local machine to see which ports between 31000 and 32000 are open and listening.



## Git Version Control

* **Repository Operations:**
* `git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo` : Downloads a remote repository. *(Note the custom port placement in the SSH URL).*


* **History & Navigation:**
* `git log` : Shows the commit history.
* `git show [hash]` : Shows the exact file changes made in a specific commit or tag (e.g., `git show secret`).
* `git branch -a` : Lists all local and remote branches.
* `git switch dev` : Switches your working directory to the `dev` branch.
* `git tag` : Lists all hidden tags/bookmarks in the repository history.


* **Committing Changes:**
1. `git add key.txt` : Stages the file for a commit.
2. `git commit -m "commiting"` : Saves the staged changes to your local history with a message.
3. `git push origin master` : Uploads the committed changes to the remote `master` branch.



## Shell Environments & Scripting

* **Bash Loops:**
* `for i in {0000..9999}; do echo "[PREVIOUS_PASSWORD] $i"; done | nc localhost 30002` : A one-liner script that generates numbers from 0000 to 9999, formats them with a password, and pipes the entire output into a network connection for brute-forcing.


* **Escaping Restricted Environments:**
* `:set shell=/bin/bash` : In the `vi`/`vim` text editor, changes the default shell variable to bash.
* `:sh` : Spawns that shell from inside `vi`.
* `$0` : A variable representing the name of the running process. Executing it respawns the default shell (useful for breaking out of modified/restricted shell environments).



---

## Flag Glossary

A breakdown of every command-line flag used in your notes:

| Command | Flag | Meaning | Explanation |
| --- | --- | --- | --- |
| `ls` | **`-a`** | **A**ll | Shows all files in a directory, including hidden ones that start with a dot (e.g., `.bashrc`). |
| `ssh` / `nmap` | **`-p`** | **P**ort | Specifies which port number the command should target instead of the default port. |
| `find` | **`-type`** | Type | Filters results by file type. `-type f` restricts the search specifically to regular files (ignoring directories or symlinks). |
| `find` | **`-size`** | Size | Filters results by file size. The `c` suffix (e.g., `1033c`) stands for bytes/characters. |
| `find` | **`!`** | NOT | A logical operator that reverses the next condition. `! -executable` means "find files that are *not* executable". |
| `find` | **`-executable`** | Executable | Filters for files that the current user has permission to execute as a program. |
| `uniq` | **`-u`** | **U**nique | Modifies `uniq` to strictly print *only* the lines that appear exactly once, entirely dropping lines that have duplicates. |
| `base64` | **`-d`** | **D**ecode | Tells the `base64` utility to decode the inputted string back to its original data, rather than encoding it. |
| `xxd` | **`-r`** | **R**everse | Reverses the hex-dump process, taking human-readable hexadecimal text and converting it back into raw binary machine code. |
| `gzip` | **`-d`** | **D**ecompress | Instructs the gzip utility to unzip/decompress the archive. |
| `tar` | **`-x`** | e**X**tract | Instructs `tar` to unpack the contents of an archive file. |
| `tar` | **`-c`** | **C**reate | Instructs `tar` to create a brand new archive. |
| `tar` | **`-f`** | **F**ile | Specifies the filename of the archive you want to create or extract. Always comes last in the flag list (e.g., `xf filename.tar`). |
| `ncat` | **`--ssl`** | SSL/TLS | Forces the network connection to use SSL/TLS encryption, required when talking to secure ports. |
| `git commit` | **`-m`** | **M**essage | Allows you to pass a commit message directly in the command line inside quotes, rather than opening a text editor. |
| `git branch` | **`-a`** | **A**ll | Lists *all* branches in a git repository, including remote-tracking branches that might be hidden by default. |
| `nc` | **`-l`** | **L**istens | set nc up and listens on a port. |




