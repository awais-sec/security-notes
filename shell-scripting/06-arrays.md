# Arrays in Shell Scripting

## 1. What Are Arrays?

An array is a collection of data elements, where each element is identified by an index. Arrays are useful when you need to manage multiple values using a single variable.

**Key characteristics:**

1. **Indexed elements** — array elements are accessed using their index. Indexing starts from 0.
2. **Dynamic** — arrays in shell scripting do not need to be predefined with a size.
3. **Homogeneous or heterogeneous** — arrays can store strings, numbers, or a mix of both.
4. **One-dimensional** — shell scripting supports only one-dimensional arrays.

## 2. Declaring and Initializing Arrays

**a. Declaring an empty array**

```bash
array_name=()
```

**b. Declaring and initializing an array**

```bash
# Method 1: Using space-separated values
array_name=("value1" "value2" "value3")

# Method 2: Adding elements individually
array_name[0]="value1"
array_name[1]="value2"
array_name[2]="value3"
```

**c. Accessing array elements**

```bash
# Access a specific element
echo "${array_name[1]}"

# Access all elements
echo "${array_name[@]}"
```

**Example:**

```bash
fruits=("apple" "banana" "cherry")
echo "First fruit: ${fruits[0]}"
echo "All fruits: ${fruits[@]}"
```

Output:

```
First fruit: apple
All fruits: apple banana cherry
```

## 3. Array Operations

**a. Adding elements**

```bash
array_name+=("new_value")
```

Example:

```bash
languages=("Python" "Java")
languages+=("Bash")
echo "${languages[@]}"
# Output: Python Java Bash
```

**b. Removing elements**

```bash
unset array_name[index]
```

Example:

```bash
numbers=(10 20 30 40)
unset numbers[1]   # Removes the second element
echo "${numbers[@]}"
# Output: 10 30 40
```

**c. Counting elements**

```bash
count=${#array_name[@]}
```

Example:

```bash
files=("file1.txt" "file2.txt" "file3.txt")
echo "Number of files: ${#files[@]}"
# Output: 3
```

**d. Modifying elements**

```bash
array_name[index]="new_value"
```

Example:

```bash
fruits=("apple" "banana" "cherry")
fruits[1]="orange"
echo "${fruits[@]}"
# Output: apple orange cherry
```

## 4. Iterating Through Arrays

**Using a for loop**

```bash
for element in "${array_name[@]}"; do
    echo "$element"
done
```

Example:

```bash
colors=("red" "green" "blue")
for color in "${colors[@]}"; do
    echo "$color"
done
```

Output:

```
red
green
blue
```

**Using index-based access**

```bash
for i in "${!array_name[@]}"; do
    echo "Index $i: ${array_name[$i]}"
done
```

Example:

```bash
numbers=(5 10 15)
for i in "${!numbers[@]}"; do
    echo "Index $i: ${numbers[$i]}"
done
```

Output:

```
Index 0: 5
Index 1: 10
Index 2: 15
```

## 5. Advanced Array Manipulations

**a. Searching for an element**

```bash
search="banana"
found=0
for item in "${fruits[@]}"; do
    if [[ "$item" == "$search" ]]; then
        found=1
        break
    fi
done

if [[ $found -eq 1 ]]; then
    echo "$search found in array."
else
    echo "$search not found."
fi
```

This loops through the array and checks if the element exists using pattern matching.

**b. Sorting an array**

Shell scripting has no built-in sort for arrays, but you can pipe through the `sort` command.

```bash
sorted=($(for item in "${array_name[@]}"; do echo "$item"; done | sort))
```

Example:

```bash
numbers=(30 10 20 40)
sorted_numbers=($(for num in "${numbers[@]}"; do echo $num; done | sort -n))
echo "Sorted: ${sorted_numbers[@]}"
```

Output:

```
Sorted: 10 20 30 40
```

**c. Slicing an array**

You can extract a portion of an array using slicing.

```bash
sliced=("${array_name[@]:start:length}")
```

Example:

```bash
files=("file1" "file2" "file3" "file4")
sliced=("${files[@]:1:2}")
echo "${sliced[@]}"
# Output: file2 file3
```

## 6. Multi-Word Strings in Arrays

Wrap multi-word strings in double quotes to store them as single elements.

```bash
quotes=("Be yourself" "Never give up" "Stay positive")
for quote in "${quotes[@]}"; do
    echo "$quote"
done
```

Output:

```
Be yourself
Never give up
Stay positive
```

## 7. Practical Applications in DFCS

**a. Find suspicious files**

```bash
files=("image.png" "program.exe" "report.docx" "malware.exe")
echo "Suspicious files:"
for file in "${files[@]}"; do
    if [[ "$file" == *.exe ]]; then
        echo "$file"
    fi
done
```

Output:

```
Suspicious files:
program.exe
malware.exe
```

**b. Compare expected and actual files**

```bash
expected_logs=("system.log" "auth.log" "cron.log")
actual_logs=("system.log" "cron.log")

echo "Missing logs:"
for log in "${expected_logs[@]}"; do
    if [[ ! " ${actual_logs[@]} " =~ " $log " ]]; then
        echo "$log"
    fi
done
```

Output:

```
Missing logs:
auth.log
```

## 8. Common Mistakes to Avoid

**a. Missing quotes**

Always quote array expansions to handle elements containing spaces.

```bash
# Correct
for item in "${array_name[@]}"; do
    echo "$item"
done
```

**b. Using incorrect syntax for array declaration**

Incorrect:

```bash
my_array="value1 value2 value3"
```

Correct:

```bash
my_array=("value1" "value2" "value3")
```

## 9. Summary

- **Declare arrays** — use parentheses `()` with space-separated values.
- **Access elements** — use `${array_name[index]}` for a specific element.
- **Manipulate arrays** — use `+=` to add, `unset` to remove, and `${#array_name[@]}` to count elements.
- **Iterate over arrays** — use `for` loops to process elements efficiently.
- **Practical applications** — arrays are useful for processing logs, managing file collections, and automating repetitive tasks.
