# Wildcard Expansion in Shell Scripting (Globbing)

Wildcard expansion, also known as globbing, is a shell scripting feature that lets you match filenames and directories using special characters instead of typing out exact names. It's especially useful when working with multiple files or directories at once.

## Wildcards in Shell Scripting

Wildcards are special symbols used in shell commands to represent one or more characters. The shell expands these wildcards into matching filenames before the command actually runs.

## Common Wildcards and Their Usage

### 1. Asterisk (`*`)

Matches zero or more characters, including no character at all. Used to select all files, or files that share a common pattern.

```bash
ls *.txt
```

Lists all `.txt` files in the current directory. Matches `file1.txt`, `notes.txt`, `data.txt`, etc.

```bash
rm *   # Deletes all files in the current directory (use with caution)
```

### 2. Question Mark (`?`)

Matches exactly one character.

```bash
ls file?.txt
```

Matches `file1.txt`, `file2.txt`, but not `file10.txt` or `file.txt`.

### 3. Square Brackets (`[ ]`)

Matches any single character inside the brackets.

```bash
ls file[123].txt
```

Matches `file1.txt`, `file2.txt`, `file3.txt`, but not `file4.txt`.

**Character ranges:**

- `[a-z]` matches any lowercase letter.
- `[A-Z]` matches any uppercase letter.
- `[0-9]` matches any digit.

```bash
ls [A-C]*.txt
```

Matches files starting with A, B, or C, followed by any characters, ending in `.txt`.

### 4. Brace Expansion (`{ }`)

Matches multiple specified values.

```bash
ls {file1,file2,file3}.txt
```

Expands to: `ls file1.txt file2.txt file3.txt`.

```bash
mkdir {Jan,Feb,Mar}
```

Creates directories `Jan`, `Feb`, and `Mar`.

### 5. Tilde (`~`)

Represents the home directory of the current user.

```bash
cd ~
```

Changes to the home directory (`/home/user` on Linux).

```bash
ls ~/Documents
```

Lists files in the `Documents` folder inside the home directory.

### 6. Double Asterisk (`**`)

Matches files and directories recursively (Bash 4.0+ only).

```bash
ls **/*.txt
```

Searches for `.txt` files in the current directory and all subdirectories.

## How Wildcard Expansion Works

1. **Pattern matching** — the shell scans the command line for wildcard patterns.
2. **Expansion** — the shell replaces the pattern with matching filenames.
3. **Execution** — the final, expanded command is executed.

Example:

```bash
echo *.sh
```

If the current directory contains `script1.sh` and `script2.sh`, the shell expands this to:

```bash
echo script1.sh script2.sh
```

## Practical Uses of Wildcard Expansion

**List specific files**

```bash
ls *.jpg
```

Lists all `.jpg` files.

**Copy multiple files**

```bash
cp file?.txt backup/
```

Copies `file1.txt`, `file2.txt`, etc., to the `backup/` directory.

**Delete specific files**

```bash
rm file[1-3].txt
```

Deletes `file1.txt`, `file2.txt`, and `file3.txt`.

**Move files based on patterns**

```bash
mv {report,summary}.pdf Documents/
```

Moves `report.pdf` and `summary.pdf` to `Documents/`.

## Wildcards vs. Regular Expressions

| Feature | Wildcards (Globbing) | Regular Expressions |
|---|---|---|
| Used in | File and directory matching | Text processing (e.g., `grep`, `sed`, `awk`) |
| Expansion | Shell expands wildcards before executing commands | Tools process regex patterns during execution |
| Characters used | `*`, `?`, `[ ]`, `{ }` | `^`, `$`, `.*`, `[ ]`, `+`, `?`, `{ }` |
