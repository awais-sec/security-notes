# Loops in Shell Scripting

## 1. Introduction to Loops

Loops allow repetitive execution of code based on a condition or over a specific range of values. In shell scripting, loops are essential for automating repetitive tasks like processing files, iterating through lists, and running continuous checks.

## 2. Types of Loops in Shell Scripting

### a. for Loop

A `for` loop iterates over a list of items or a range of numbers.

**Syntax:**

```bash
for item in list; do
    # Code to execute for each item
done
```

**Example:**

```bash
for i in {1..5}; do
    echo "Number: $i"
done
```

The loop runs 5 times, printing each number from 1 to 5.

### b. while Loop

A `while` loop executes code as long as a specified condition is true.

**Syntax:**

```bash
while [ condition ]; do
    # Code to execute while condition is true
done
```

**Example:**

```bash
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

This loop prints "Count" followed by the current value and increments until `count` reaches 5.

### c. until Loop

An `until` loop executes code until a specified condition becomes true (the opposite of `while`).

**Syntax:**

```bash
until [ condition ]; do
    # Code to execute until condition is true
done
```

**Example:**

```bash
num=1
until [ $num -gt 5 ]; do
    echo "Number: $num"
    num=$((num + 1))
done
```

The loop increments `num` and stops once it becomes greater than 5.

## 3. Loop Control Commands

Shell scripting provides commands to control the flow within loops:

- `break`: Exits the loop immediately.
- `continue`: Skips the current iteration and proceeds to the next.

**Example with `break`:**

```bash
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo "Number: $i"
done
```

**Example with `continue`:**

```bash
for i in {1..5}; do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo "Number: $i"
done
```

## 4. Practical Examples of Loops

### Example 1: Print Each Word in a Sentence

```bash
sentence="Shell scripting is powerful"
for word in $sentence; do
    echo "$word"
done
```

### Example 2: Count Down from 10 to 1

```bash
count=10
while [ $count -gt 0 ]; do
    echo "$count"
    count=$((count - 1))
done
```

### Example 3: Run a Command Until a Condition is Met

```bash
num=1
until [ $num -gt 3 ]; do
    echo "Current number: $num"
    num=$((num + 1))
done
```

## 5. Practice Tasks

### Task 1: List All .exe and .bat Files in a Directory

```bash
for file in downloads/*.{exe,bat}; do
    if [ -e "$file" ]; then
        echo "Suspicious file: $file"
    fi
done
```

Lists potentially harmful files in the downloads folder.

### Task 2: Scan a Log File for Unauthorized Access Attempts

```bash
while read -r line; do
    if echo "$line" | grep -q "unauthorized"; then
        echo "Unauthorized access: $line"
    fi
done < server.log
```

Prints each line containing "unauthorized" from `server.log`.

### Task 3: Check Disk Usage of Important Directories

```bash
for dir in /var/log /home/user/forensics; do
    usage=$(df "$dir" | awk 'NR==2 {print $5}' | sed 's/%//')
    if [ "$usage" -gt 80 ]; then
        echo "Warning: $dir usage is at $usage%"
    fi
done
```

Warns if any specified directory exceeds 80% disk usage.

### Task 4: Find Large Files in a Directory

```bash
for file in $(find forensics -type f -size +100M); do
    echo "Large file: $file"
done
```

Lists files over 100MB in the `forensics` directory.

### Task 5: Monitor Active Network Connections

```bash
count=0
while [ $count -lt 3 ]; do
    echo "Active network connections:"
    netstat -tu
    count=$((count + 1))
    sleep 5
done
```

Prints network connections every 5 seconds, repeating 3 times.

## 6. Practice Exercises

### Exercise 1: Print Odd Numbers from 1 to 15 Using a for Loop

```bash
for i in {1..15..2}; do
    echo "$i"
done
```

### Exercise 2: Calculate Sum of Numbers from 1 to 10

```bash
sum=0
for i in {1..10}; do
    sum=$((sum + i))
done
echo "Sum: $sum"
```

### Exercise 3: Check if Important Files Are Readable

```bash
for file in /etc/passwd /etc/shadow; do
    if [ ! -r "$file" ]; then
        echo "$file is not readable"
    else
        echo "$file is readable"
    fi
done
```
