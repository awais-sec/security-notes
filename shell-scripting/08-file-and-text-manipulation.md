# File & Text Manipulation

## 1. Introduction to File and Text Manipulation

File and text manipulation is an essential part of shell scripting, enabling users to process, analyze, and modify files and their contents efficiently. Shell scripting provides a variety of tools for these tasks, including `cat`, `grep`, `awk`, `sed`, `cut`, `sort`, `uniq`, and more.

## 2. Commonly Used Commands for File and Text Manipulation

| Command | Description |
|---|---|
| `cat` | Displays or combines file contents |
| `grep` | Searches for text patterns in a file |
| `awk` | Processes and formats text, often structured data |
| `sed` | Edits text in a file or stream |
| `cut` | Extracts specific fields or columns from a file |
| `sort` | Sorts lines of text alphabetically or numerically |
| `uniq` | Removes duplicate lines |
| `wc` | Counts lines, words, and characters in a file |
| `head` | Displays the first few lines of a file |
| `tail` | Displays the last few lines of a file |
| `tr` | Transforms or deletes characters |

## 3. File Manipulation Tasks

### 3.1 Counting Lines in a File

**Task:** Count the number of lines in a file.

```bash
#!/bin/bash
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    lines=$(wc -l < "$file")
    echo "Number of lines in $file: $lines"
else
    echo "File not found!"
fi
```

### 3.2 Displaying the First Few Lines of a File

**Task:** Display the first 5 lines of a file.

```bash
#!/bin/bash
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    head -n 5 "$file"
else
    echo "File not found!"
fi
```

### 3.3 Merging Two Files

**Task:** Merge the contents of two files into a new file.

```bash
#!/bin/bash
read -p "Enter the first file: " file1
read -p "Enter the second file: " file2
read -p "Enter the output file: " output

if [[ -f "$file1" && -f "$file2" ]]; then
    cat "$file1" "$file2" > "$output"
    echo "Files merged into $output."
else
    echo "One or both files not found!"
fi
```

### 3.4 Searching for a Word in a File

**Task:** Search for a specific word in a file.

```bash
#!/bin/bash
read -p "Enter the word to search: " word
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    grep -i "$word" "$file"
else
    echo "File not found!"
fi
```

### 3.5 Converting Text to Uppercase

**Task:** Convert the contents of a file to uppercase.

```bash
#!/bin/bash
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    cat "$file" | tr '[:lower:]' '[:upper:]'
else
    echo "File not found!"
fi
```

## 4. Text Processing Tasks

### 4.1 Extracting Specific Columns from a File

**Task:** Extract specific columns from a file.

```bash
#!/bin/bash
read -p "Enter the file name: " file
read -p "Enter the column numbers (e.g., 1,2): " columns
if [[ -f "$file" ]]; then
    cut -d ' ' -f "$columns" "$file"
else
    echo "File not found!"
fi
```

### 4.2 Sorting and Removing Duplicates

**Task:** Sort a file and remove duplicate lines.

```bash
#!/bin/bash
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    sort "$file" | uniq
else
    echo "File not found!"
fi
```

### 4.3 Replacing Text in a File

**Task:** Replace all occurrences of a word in a file.

```bash
#!/bin/bash
read -p "Enter the file name: " file
read -p "Enter the word to replace: " old_word
read -p "Enter the new word: " new_word
if [[ -f "$file" ]]; then
    sed -i "s/$old_word/$new_word/g" "$file"
    echo "Replaced '$old_word' with '$new_word' in $file."
else
    echo "File not found!"
fi
```

### 4.4 Counting Words and Characters

**Task:** Count the number of words and characters in a file.

```bash
#!/bin/bash
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    words=$(wc -w < "$file")
    chars=$(wc -m < "$file")
    echo "Words: $words, Characters: $chars"
else
    echo "File not found!"
fi
```

### 4.5 Appending Timestamps to Lines

**Task:** Append the current timestamp to each line in a file.

```bash
#!/bin/bash
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    awk '{print $0, strftime("%Y-%m-%d %H:%M:%S")}' "$file"
else
    echo "File not found!"
fi
```

## 5. Advanced Text Manipulation Tasks

### 5.1 Analyzing Log Files

**Task:** Count the number of "ERROR" lines in a log file.

```bash
#!/bin/bash
read -p "Enter the log file: " log_file
if [[ -f "$log_file" ]]; then
    errors=$(grep -c "ERROR" "$log_file")
    echo "Number of 'ERROR' lines: $errors"
else
    echo "Log file not found!"
fi
```

### 5.2 Extracting Unique File Extensions

**Task:** Extract and list all unique file extensions in a directory.

```bash
#!/bin/bash
read -p "Enter the directory: " dir
if [[ -d "$dir" ]]; then
    find "$dir" -type f | awk -F. '{if (NF>1) print $NF}' | sort | uniq
else
    echo "Directory not found!"
fi
```

### 5.3 Generating a Word Frequency Report

**Task:** Count the frequency of each word in a file and display the top 5.

```bash
#!/bin/bash
read -p "Enter the file name: " file
if [[ -f "$file" ]]; then
    tr '[:space:]' '[\n*]' < "$file" | grep -v '^$' | sort | uniq -c | sort -nr | head -n 5
else
    echo "File not found!"
fi
```

### 5.4 Replacing Text in Multiple Files

**Task:** Replace text in all `.txt` files in a directory.

```bash
#!/bin/bash
read -p "Enter the directory: " dir
read -p "Enter the text to replace: " old_text
read -p "Enter the new text: " new_text
if [[ -d "$dir" ]]; then
    find "$dir" -type f -name "*.txt" -exec sed -i "s/$old_text/$new_text/g" {} +
    echo "Text replaced in all .txt files in $dir."
else
    echo "Directory not found!"
fi
```

## 6. Summary

**Key concepts covered:**

- File content display (`cat`, `head`, `tail`)
- Searching and replacing (`grep`, `sed`)
- Counting lines, words, and characters (`wc`)
- Extracting specific data (`cut`, `awk`)
- Sorting and removing duplicates (`sort`, `uniq`)
