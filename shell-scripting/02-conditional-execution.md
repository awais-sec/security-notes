# Conditional Execution in Shell Scripting

## 1. Introduction to Conditional Execution

Conditional execution allows scripts to make decisions based on specific conditions, which is a fundamental part of programming. Using `if`, `elif`, `else`, and related constructs, a script can branch its behavior depending on the situation it encounters.

## 2. Basic Syntax of Conditional Statements

The basic structure of an `if` statement in shell scripting is:

```bash
if [ condition ]; then
    # Code to execute if condition is true
fi
```

- `if`: Starts the conditional statement.
- `[ condition ]`: The condition to evaluate.
- `then`: Indicates the start of the code block for a true condition.
- `fi`: Ends the `if` statement.

## 3. Common Conditional Operators

### File Tests

| Operator | Description | Example |
|---|---|---|
| `-e` | File exists | `[ -e file.txt ]` |
| `-f` | Is a regular file | `[ -f file.txt ]` |
| `-d` | Is a directory | `[ -d /home/user/ ]` |
| `-r` | Has read permission | `[ -r file.txt ]` |
| `-w` | Has write permission | `[ -w file.txt ]` |
| `-x` | Has execute permission | `[ -x file.sh ]` |

### Numeric Comparisons

| Operator | Description | Example |
|---|---|---|
| `-eq` | Equal | `[ $a -eq $b ]` |
| `-ne` | Not equal | `[ $a -ne $b ]` |
| `-gt` | Greater than | `[ $a -gt $b ]` |
| `-lt` | Less than | `[ $a -lt $b ]` |
| `-ge` | Greater than or equal to | `[ $a -ge $b ]` |
| `-le` | Less than or equal to | `[ $a -le $b ]` |

### String Comparisons

| Operator | Description | Example |
|---|---|---|
| `=` | String equality | `[ "$a" = "$b" ]` |
| `!=` | String inequality | `[ "$a" != "$b" ]` |
| `-z` | String is empty | `[ -z "$a" ]` |
| `-n` | String is not empty | `[ -n "$a" ]` |

## 4. if-else Statement

Adds an alternative block of code that runs when the condition is false.

```bash
if [ condition ]; then
    # Code if condition is true
else
    # Code if condition is false
fi
```

Example:

```bash
#!/bin/bash
if [ -e "file.txt" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

## 5. if-elif-else Statement

Allows multiple conditions to be checked in sequence.

```bash
if [ condition1 ]; then
    # Code if condition1 is true
elif [ condition2 ]; then
    # Code if condition2 is true
else
    # Code if all conditions are false
fi
```

Example:

```bash
#!/bin/bash
number=5
if [ "$number" -gt 10 ]; then
    echo "Greater than 10"
elif [ "$number" -eq 10 ]; then
    echo "Equal to 10"
else
    echo "Less than 10"
fi
```

## 6. Nested if Statements

An `if` statement can be placed inside another `if` statement to handle more complex conditions.

Example:

```bash
#!/bin/bash
if [ -e "file.txt" ]; then
    if [ -r "file.txt" ]; then
        echo "File exists and is readable."
    else
        echo "File exists but is not readable."
    fi
else
    echo "File does not exist."
fi
```

## 7. Logical Operators

Logical operators combine multiple conditions within a single `if` statement.

| Operator | Description | Example |
|---|---|---|
| `&&` | Logical AND | `[ condition1 ] && [ condition2 ]` |
| `\|\|` | Logical OR | `[ condition1 ] \|\| [ condition2 ]` |

Example:

```bash
#!/bin/bash
if [ -e "file.txt" ] && [ -w "file.txt" ]; then
    echo "File exists and is writable."
else
    echo "File does not exist or is not writable."
fi
```

## 8. Examples and Practice Tasks

### Example 1: Check for Even or Odd

```bash
#!/bin/bash
echo "Enter a number:"
read number
if [ $((number % 2)) -eq 0 ]; then
    echo "The number is even."
else
    echo "The number is odd."
fi
```

### Example 2: Check File Permissions

```bash
#!/bin/bash
if [ -r "file.txt" ]; then
    echo "File is readable."
fi
if [ -w "file.txt" ]; then
    echo "File is writable."
fi
if [ -x "file.txt" ]; then
    echo "File is executable."
fi
```

### Practice Tasks

1. **Check User Input** — Write a script that takes user input and prints "Empty input" if the input is empty.
2. **File Type Check** — Write a script to check if a given path is a file or a directory.
3. **Simple Calculator** — Write a script that takes two numbers and an operator as inputs, then performs the corresponding arithmetic operation.
