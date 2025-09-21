# Jq
A comprehensive guide/cheatsheet for learning jq command for data processing in linux
# Comprehensive jq Learning Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Basic Syntax](#basic-syntax)
4. [Core Concepts](#core-concepts)
5. [Basic Operations](#basic-operations)
6. [Filtering and Selection](#filtering-and-selection)
7. [Array Operations](#array-operations)
8. [Object Operations](#object-operations)
9. [Functions and Operators](#functions-and-operators)
10. [Advanced Features](#advanced-features)
11. [Common Use Cases](#common-use-cases)
12. [Best Practices](#best-practices)
13. [Troubleshooting](#troubleshooting)
14. [Resources](#resources)

## Introduction

`jq` is a lightweight and flexible command-line JSON processor. It's like `sed` for JSON data - you can use it to slice, filter, map, and transform structured data with ease.

### What can jq do?
- Parse and pretty-print JSON
- Extract specific values from complex JSON structures
- Transform JSON data into different formats
- Filter arrays and objects based on conditions
- Perform calculations on JSON data
- Combine multiple JSON documents

## Installation

### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install jq
```

### CentOS/RHEL/Fedora
```bash
# CentOS/RHEL
sudo yum install jq
# or
sudo dnf install jq

# Fedora
sudo dnf install jq
```

### macOS
```bash
brew install jq
```

### From Source
```bash
git clone https://github.com/stedolan/jq.git
cd jq
git submodule update --init
autoreconf -fi
./configure
make
sudo make install
```

### Verify Installation
```bash
jq --version
```

## Basic Syntax

The basic syntax of jq is:
```bash
jq [options] 'filter' [file...]
```

### Common Options
- `-r, --raw-output`: Output raw strings, not JSON texts
- `-n, --null-input`: Use null as input value
- `-e, --exit-status`: Set exit status based on output
- `-s, --slurp`: Read entire input stream into array
- `-c, --compact-output`: Compact instead of pretty-printed output
- `-M, --monochrome-output`: Disable colored output
- `-a, --ascii-output`: Force ASCII output

## Core Concepts

### Identity Filter (.)
The simplest filter is `.` which returns the input unchanged:
```bash
echo '{"name": "John", "age": 30}' | jq '.'
```
Output:
```json
{
  "name": "John",
  "age": 30
}
```

### Field Access
Access object fields using `.fieldname`:
```bash
echo '{"name": "John", "age": 30}' | jq '.name'
# Output: "John"
```

### Array Indexing
Access array elements using `.[index]`:
```bash
echo '[1, 2, 3, 4, 5]' | jq '.[2]'
# Output: 3
```

### Pipe Operator (|)
Chain operations using the pipe operator:
```bash
echo '{"user": {"name": "John"}}' | jq '.user | .name'
# Output: "John"
```

## Basic Operations

### Pretty Printing
```bash
echo '{"name":"John","age":30,"city":"NYC"}' | jq '.'
```

### Raw Output
```bash
echo '{"message": "Hello World"}' | jq -r '.message'
# Output: Hello World (without quotes)
```

### Multiple Fields
```bash
echo '{"name": "John", "age": 30, "city": "NYC"}' | jq '.name, .age'
# Output:
# "John"
# 30
```

### Nested Field Access
```bash
echo '{"user": {"profile": {"name": "John"}}}' | jq '.user.profile.name'
# Output: "John"
```

## Filtering and Selection

### Select Function
Filter objects based on conditions:
```bash
echo '[{"name": "John", "age": 30}, {"name": "Jane", "age": 25}]' | jq '.[] | select(.age > 27)'
```

### Has Function
Check if object has a key:
```bash
echo '{"name": "John", "age": 30}' | jq 'has("age")'
# Output: true
```

### Type Function
Get the type of a value:
```bash
echo '{"name": "John", "age": 30}' | jq '.age | type'
# Output: "number"
```

### Empty Values
Filter out null/empty values:
```bash
echo '[1, null, 2, "", 3]' | jq '.[] | select(. != null and . != "")'
```

## Array Operations

### Array Iteration (.[] )
Iterate over array elements:
```bash
echo '[1, 2, 3]' | jq '.[]'
# Output:
# 1
# 2
# 3
```

### Array Slicing
```bash
echo '[1, 2, 3, 4, 5]' | jq '.[1:3]'
# Output: [2, 3]
```

### Array Length
```bash
echo '[1, 2, 3, 4, 5]' | jq 'length'
# Output: 5
```

### Map Function
Transform each element:
```bash
echo '[1, 2, 3]' | jq 'map(. * 2)'
# Output: [2, 4, 6]
```

### Sort and Sort_by
```bash
echo '[3, 1, 4, 1, 5]' | jq 'sort'
# Output: [1, 1, 3, 4, 5]

echo '[{"name": "John", "age": 30}, {"name": "Jane", "age": 25}]' | jq 'sort_by(.age)'
```

### Unique
```bash
echo '[1, 2, 2, 3, 3, 3]' | jq 'unique'
# Output: [1, 2, 3]
```

### Group_by
```bash
echo '[{"type": "A", "value": 1}, {"type": "B", "value": 2}, {"type": "A", "value": 3}]' | jq 'group_by(.type)'
```

### Min/Max
```bash
echo '[1, 5, 3, 9, 2]' | jq 'min'
# Output: 1

echo '[1, 5, 3, 9, 2]' | jq 'max'
# Output: 9
```

### Add (Sum/Concatenate)
```bash
echo '[1, 2, 3, 4, 5]' | jq 'add'
# Output: 15

echo '["hello", " ", "world"]' | jq 'add'
# Output: "hello world"
```

## Object Operations

### Keys
Get object keys:
```bash
echo '{"name": "John", "age": 30, "city": "NYC"}' | jq 'keys'
# Output: ["age", "city", "name"]
```

### Values
Get object values:
```bash
echo '{"name": "John", "age": 30}' | jq '.[]'
# or
echo '{"name": "John", "age": 30}' | jq 'values'
```

### Object Construction
Create new objects:
```bash
echo '{"first": "John", "last": "Doe"}' | jq '{name: (.first + " " + .last), full: .}'
```

### With_entries
Transform key-value pairs:
```bash
echo '{"a": 1, "b": 2}' | jq 'with_entries(.value *= 2)'
# Output: {"a": 2, "b": 4}
```

### To_entries/From_entries
Convert between object and key-value pairs:
```bash
echo '{"name": "John", "age": 30}' | jq 'to_entries'
# Output: [{"key": "name", "value": "John"}, {"key": "age", "value": 30}]
```

## Functions and Operators

### String Functions
```bash
# Length
echo '"hello"' | jq 'length'
# Output: 5

# Split
echo '"hello,world"' | jq 'split(",")'
# Output: ["hello", "world"]

# Join
echo '["hello", "world"]' | jq 'join(" ")'
# Output: "hello world"

# Contains
echo '"hello world"' | jq 'contains("world")'
# Output: true

# Starts with / Ends with
echo '"hello world"' | jq 'startswith("hello")'
# Output: true

# Case conversion
echo '"Hello World"' | jq 'ascii_downcase'
# Output: "hello world"
```

### Math Functions
```bash
# Floor/Ceil/Round
echo '3.7' | jq 'floor'
# Output: 3

echo '3.2' | jq 'ceil'
# Output: 4

echo '3.6' | jq 'round'
# Output: 4

# Absolute value
echo '-5' | jq 'abs'
# Output: 5
```

### Date Functions
```bash
# Current timestamp
echo 'null' | jq 'now'

# Convert timestamp to date
echo '1577836800' | jq 'todate'

# Convert date to timestamp
echo '"2020-01-01T00:00:00Z"' | jq 'fromdate'
```

### Logical Operators
```bash
# AND
echo '{"a": true, "b": true}' | jq '.a and .b'
# Output: true

# OR  
echo '{"a": true, "b": false}' | jq '.a or .b'
# Output: true

# NOT
echo 'true' | jq 'not'
# Output: false
```

## Advanced Features

### Conditionals (if-then-else)
```bash
echo '[1, 2, 3, 4, 5]' | jq 'map(if . > 3 then "big" else "small" end)'
# Output: ["small", "small", "small", "big", "big"]
```

### Try-Catch
Handle errors gracefully:
```bash
echo '{"name": "John"}' | jq 'try .age catch "age not found"'
# Output: "age not found"
```

### Alternative Operator (//)
Provide fallback values:
```bash
echo '{"name": "John"}' | jq '.age // 0'
# Output: 0
```

### Variables
Define and use variables:
```bash
echo '{"items": [1,2,3], "multiplier": 2}' | jq '.multiplier as $m | .items | map(. * $m)'
# Output: [2, 4, 6]
```

### Reduce
Accumulate values:
```bash
echo '[1, 2, 3, 4, 5]' | jq 'reduce .[] as $item (0; . + $item)'
# Output: 15
```

### Path Function
Get the path to a value:
```bash
echo '{"a": {"b": {"c": 42}}}' | jq 'path(.a.b.c)'
# Output: ["a", "b", "c"]
```

### Recursive Descent (..)
Search recursively:
```bash
echo '{"a": {"b": 1, "c": {"d": 2}}}' | jq '.. | numbers'
# Output: 1, 2
```

## Common Use Cases

### Configuration Files
Extract configuration values:
```bash
# Get database host from config.json
jq -r '.database.host' config.json

# Get all service ports
jq '.services[].port' config.json
```

### API Responses
Process API responses:
```bash
# Get user names from API response
curl -s 'https://api.example.com/users' | jq '.[].name'

# Extract error messages
curl -s 'https://api.example.com/endpoint' | jq '.errors[]?.message'
```

### Log Analysis
Parse JSON logs:
```bash
# Filter error logs
cat app.log | jq 'select(.level == "error")'

# Count log levels
cat app.log | jq -r '.level' | sort | uniq -c
```

### Data Transformation
Transform data format:
```bash
# Convert array of objects to CSV-like format
echo '[{"name": "John", "age": 30}, {"name": "Jane", "age": 25}]' | jq -r '.[] | "\(.name),\(.age)"'
```

### Package.json Operations
```bash
# Get dependencies
jq '.dependencies | keys[]' package.json

# Update version
jq '.version = "2.0.0"' package.json
```

## Best Practices

### Performance Tips
1. Use specific filters instead of iterating when possible
2. Filter early in the pipeline to reduce data processing
3. Use `-c` for compact output when pretty printing isn't needed
4. Consider using `--stream` for very large JSON files

### Readability
1. Use meaningful variable names with `as $variable`
2. Break complex queries into smaller, chained operations
3. Add comments using `# comment` syntax where supported
4. Use proper indentation for multi-line queries

### Error Handling
1. Use `try-catch` for potentially failing operations
2. Use `//` operator for default values
3. Check for key existence with `has()` before accessing
4. Use `select()` to filter out invalid data

### Security
1. Be cautious with user-provided input in jq expressions
2. Validate data types before processing
3. Use appropriate quoting for shell integration

## Troubleshooting

### Common Errors

**"Cannot index string with string"**
```bash
# Wrong: trying to access property on string
echo '"hello"' | jq '.length'

# Correct: use length function
echo '"hello"' | jq 'length'
```

**"Cannot iterate over null"**
```bash
# Wrong: trying to iterate when field might not exist
echo '{}' | jq '.items[]'

# Correct: use try or check existence
echo '{}' | jq '.items[]?'
# or
echo '{}' | jq 'if has("items") then .items[] else empty end'
```

**"Expected string but got array"**
```bash
# Wrong: passing array to string function
echo '[1,2,3]' | jq 'split(",")'

# Correct: convert to string first or use appropriate function
echo '[1,2,3]' | jq 'join(",")'
```

### Debugging Tips
1. Use `. | debug` to inspect intermediate values
2. Add `| type` to check data types at any point
3. Use `| length` to check array/object sizes
4. Test filters step by step by removing parts of the pipeline

## Resources

### Official Documentation
- [jq Manual](https://stedolan.github.io/jq/manual/)
- [jq Tutorial](https://stedolan.github.io/jq/tutorial/)

### Online Tools
- [jq Playground](https://jqplay.org/) - Test jq expressions online
- [JSON Formatter & Validator](https://jsonformatter.curiousconcept.com/)

### Examples and Tutorials
- [jq Cookbook](https://github.com/stedolan/jq/wiki/Cookbook)
- [Advanced jq](https://github.com/stedolan/jq/wiki/Advanced-Topics)

### Command Line Integration
```bash
# Combine with curl
curl -s 'https://api.github.com/user' | jq '.name'

# Use in scripts
#!/bin/bash
API_RESPONSE=$(curl -s 'https://api.example.com/data')
USER_COUNT=$(echo "$API_RESPONSE" | jq '.users | length')
echo "Found $USER_COUNT users"

# Pipe to other commands
jq -r '.users[].email' data.json | sort | uniq
```

### Quick Reference Card
```bash
# Basic
jq '.'                    # Pretty print
jq '.field'              # Get field
jq '.field1.field2'      # Nested field
jq '.[0]'                # Array index
jq '.[]'                 # Array iteration

# Filtering
jq '.[] | select(.age > 30)'           # Filter
jq 'map(select(.active))'              # Filter map
jq '.[] | select(has("email"))'        # Has key

# Arrays
jq 'length'              # Length
jq 'sort'                # Sort
jq 'reverse'             # Reverse
jq 'unique'              # Unique values
jq 'group_by(.type)'     # Group
jq 'map(.field)'         # Transform

# Objects
jq 'keys'                # Object keys
jq 'to_entries'          # Key-value pairs
jq '{newkey: .oldkey}'   # Reconstruct

# Output
jq -r '.field'           # Raw output
jq -c '.'                # Compact output
```

---

This guide covers the essential aspects of jq for JSON processing. Practice with real JSON data to become proficient with these powerful filtering and transformation capabilities!
