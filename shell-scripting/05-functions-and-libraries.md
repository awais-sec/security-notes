# Functions & Libraries in Shell Scripting

## Part 1: Functions

Functions in shell scripting are reusable blocks of code that perform specific tasks. They help organize code, reduce redundancy, and improve readability.

### Syntax

```bash
function function_name() {
    # Code to execute
}
```

or, equivalently:

```bash
function_name() {
    # Code to execute
}
```

### Key Points

1. **Declaration** — a function can be declared using the `function` keyword or directly with the function name.
2. **Execution** — functions are executed by calling their name.
3. **Arguments** — functions can accept arguments and access them as `$1`, `$2`, etc.
4. **Return value** — use `return` to provide an exit status, or `echo` for output.

### Examples

**1. Simple Function**

```bash
#!/bin/bash
greet() {
    echo "Hello, World!"
}
# Call the function
greet
```

Output:

```
Hello, World!
```

**2. Function with Parameters**

```bash
#!/bin/bash
greet_user() {
    echo "Hello, $1!"
}
# Call the function with an argument
greet_user "Arafat"
```

Output:

```
Hello, Arafat!
```

**3. Function Returning a Value**

```bash
#!/bin/bash
add_numbers() {
    local sum=$(( $1 + $2 ))
    echo $sum
}
result=$(add_numbers 5 10)
echo "The sum is: $result"
```

Output:

```
The sum is: 15
```

**4. Function with Conditional Logic**

```bash
#!/bin/bash
check_number() {
    if (( $1 > 0 )); then
        echo "Positive number"
    elif (( $1 < 0 )); then
        echo "Negative number"
    else
        echo "Zero"
    fi
}
check_number -5
check_number 0
check_number 10
```

Output:

```
Negative number
Zero
Positive number
```

**5. Function to Process Files**

```bash
#!/bin/bash
count_lines() {
    wc -l < "$1"
}
file="example.txt"
lines=$(count_lines "$file")
echo "The file has $lines lines."
```

## Part 2: Libraries

Libraries in shell scripting are files containing reusable functions or code that can be sourced into other scripts. They allow for modularity and code reuse.

### Creating a Library

1. Write reusable functions in a separate file.
2. Use `source` (or `.`) to include the library in another script.

### Example: Creating and Using a Library

**1. Create a Library File**

Save the following as `math_library.sh`:

```bash
#!/bin/bash
add() {
    echo $(( $1 + $2 ))
}
subtract() {
    echo $(( $1 - $2 ))
}
multiply() {
    echo $(( $1 * $2 ))
}
divide() {
    if [[ $2 -ne 0 ]]; then
        echo $(( $1 / $2 ))
    else
        echo "Division by zero is not allowed."
    fi
}
```

**2. Use the Library in a Script**

```bash
#!/bin/bash
# Source the library
source math_library.sh

# Use the functions
echo "Addition: $(add 10 5)"
echo "Subtraction: $(subtract 10 5)"
echo "Multiplication: $(multiply 10 5)"
echo "Division: $(divide 10 5)"
```

Output:

```
Addition: 15
Subtraction: 5
Multiplication: 50
Division: 2
```

## Part 3: Combining Functions and Libraries

Libraries can bundle multiple functions for specific tasks, such as managing files, processing text, or networking.

### Example: Utility Library

Save as `utility.sh`:

```bash
#!/bin/bash
count_files() {
    echo "Number of files in $1: $(ls "$1" | wc -l)"
}

search_file() {
    local file=$1
    local directory=$2
    if [[ -f "$directory/$file" ]]; then
        echo "File $file found in $directory."
    else
        echo "File $file not found in $directory."
    fi
}
```

**Using the Utility Library**

```bash
#!/bin/bash
source utility.sh

# Count files in a directory
count_files /etc

# Search for a file in a directory
search_file "hosts" "/etc"
```

Output:

```
Number of files in /etc: 100
File hosts found in /etc.
```

## Part 4: Advanced Use Cases

**1. Logging System**

```bash
#!/bin/bash
log_message() {
    local level=$1
    local message=$2
    echo "[$level] $message"
}

log_message "INFO" "Script started"
log_message "ERROR" "An error occurred"
```

**2. User Input Validator**

```bash
#!/bin/bash
validate_input() {
    local input=$1
    if [[ $input =~ ^[0-9]+$ ]]; then
        echo "Valid number"
    else
        echo "Invalid input"
    fi
}

validate_input "123"
validate_input "abc"
```

## Best Practices

1. **Use local variables** — use `local` inside functions to avoid conflicts with global variables.
2. **Error handling** — check for errors and handle them gracefully.
3. **Document functions** — add comments to explain the purpose of each function.
4. **Organize libraries** — keep related functions together in library files.
