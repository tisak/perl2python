# Control Structures

## Conditional structures (if, else, elif)

Both Perl and Python provide conditional structures that allow for branching based on evaluated conditions. Let's explore their syntax and usage in both languages.

### Perl: Conditional Structures

#### 1. **if**:
```perl
if ($condition) {
    # code to be executed if condition is true
}
```

#### 2. **if-else**:
```perl
if ($condition) {
    # code to be executed if condition is true
} else {
    # code to be executed if condition is false
}
```

#### 3. **if-elsif-else**:
```perl
if ($condition1) {
    # code to be executed if condition1 is true
} elsif ($condition2) {
    # code to be executed if condition2 is true
} else {
    # code to be executed if neither condition is true
}
```

#### Note: 
In Perl, the condition does not require parentheses `()` around it, but they're commonly used for clarity.

### Python: Conditional Structures

#### 1. **if**:
```python
if condition:
    # code to be executed if condition is true
```

#### 2. **if-else**:
```python
if condition:
    # code to be executed if condition is true
else:
    # code to be executed if condition is false
```

#### 3. **if-elif-else**:
```python
if condition1:
    # code to be executed if condition1 is true
elif condition2:
    # code to be executed if condition2 is true
else:
    # code to be executed if neither condition is true
```

#### Note: 
In Python, the colon `:` is crucial and indentation is used instead of braces `{}` for block delineation.

### Comparison:

- **Syntax**: 
  - Perl uses braces `{}` to define code blocks, whereas Python uses indentation.
  - Python requires a colon `:` after each condition.
  
- **Terminology**: 
  - Perl uses `elsif` for additional conditions, whereas Python uses `elif`.

- **Parentheses**: 
  - In Perl, parentheses `()` around conditions are optional but commonly used. In Python, they're not typical unless they clarify compound conditions.

### Truthiness

#### Table Comparison of Truthiness

| Value Type         | Perl (Truthy)             | Perl (Falsy)              | Python (Truthy)           | Python (Falsy)            |
|---------------------|---------------------------|---------------------------|---------------------------|---------------------------|
| **Numbers**         | Non-zero numbers          | 0                         | Non-zero numbers          | 0                         |
| **Strings**         | Non-empty strings         | Empty string ("")         | Non-empty strings         | Empty string ("")         |
| **Arrays/Lists**    | Non-empty arrays          | Empty arrays (`()`)       | Non-empty lists           | Empty lists (`[]`)        |
| **Hashes/Dicts**    | Non-empty hashes          | Empty hashes (`{}`)       | Non-empty dictionaries    | Empty dictionaries (`{}`) |
| **Undefined/None**  | N/A                       | `undef`                   | N/A                       | `None`                    |
| **Boolean**         | True (`1`)                | False (`""` or `0`)       | `True`                    | `False`                   |

#### Notes:

1. **Perl**: 
    - The main falsy values in Perl are `0`, the empty string, and `undef`. Most everything else evaluates to truthy.
    - Arrays and hashes evaluate to `false` when empty.

2. **Python**:
    - In Python, besides `0` and empty string, other falsy values include `None`, empty collections (like `[]`, `{}`, `set()`), and `False`.
    - Custom objects can be made to evaluate to `false` in a boolean context by defining a `__bool__()` method or `__len__()` method to return `False` or `0` respectively.

#### Reference Truthiness

In Perl, a reference to an empty hash or array is truthy. The reference itself, regardless of what it points to, has a truthy value.

In Python, references (or, more correctly, variables) point to objects. The truthiness of a variable in a boolean context is determined by the object it points to, not the reference itself. Here's how it works:

- If a variable points to an empty list or dictionary, it will be falsy.
- If a variable points to a non-empty list or dictionary, it will be truthy.

## Loop structures: for and while loops

Loop structures are fundamental in almost every programming language, allowing for repeated execution of blocks of code. Both Perl and Python offer `for` and `while` loops, but their behavior and syntax differ.

## 1. `for` Loops:

### Perl:
- **List-based `for` loop**:
  ```perl
  for my $item (@array) {
      print $item;
  }
  ```

- **Range-based `for` loop**:
  ```perl
  for my $i (0..4) {
      print $i;  # Prints numbers from 0 to 4
  }
  ```

### Python:
- **List-based `for` loop**:
  ```python
  for item in list:
      print(item)
  ```

- **Range-based `for` loop**:
  ```python
  for i in range(5):
      print(i)  # Prints numbers from 0 to 4
  ```

**Nuances**:
- In Perl, the `..` operator is used to create a range, whereas in Python, the `range()` function is used.
- In Perl, the variable (`$item` or `$i` in the above examples) does not retain its value outside the loop, unless it was previously declared. In Python, the loop variable retains its value outside the loop.

## 2. `while` Loops:

### Perl:
```perl
while ($condition) {
    # code to be executed
}
```

### Python:
```python
while condition:
    # code to be executed
```

**Nuances**:
- In both languages, the `while` loop continues as long as the condition is truthy.
- One common use of the `while` loop is to iterate over file lines or other input streams.
- The syntax is very similar in both languages, with key differences being in block delineation: Perl uses curly braces `{}`, while Python uses indentation.

### Other Loop-related Nuances:

- **Loop Control Statements**:
  - **last/next in Perl** vs. **break/continue in Python**: In Perl, you use `last` to break out of a loop and `next` to skip the rest of the current iteration. In Python, you'd use `break` and `continue`, respectively.
  
- **Loop Modifiers in Perl**: Perl has a unique feature where you can place conditions after a statement to create a loop or conditional execution, e.g., `print while <$filehandle>;`.

- **Python's `else` with Loops**: Python has a somewhat unusual feature where you can use an `else` block with loops (`for` or `while`). This `else` block runs after the loop finishes (i.e., once the loop condition becomes falsy), but not if the loop was terminated by a `break` statement.
  - **`for` Loop with `else`:**
    Imagine you're searching for an item in a list, and you want to notify if the item isn't found:

    ```python
    items = [1, 2, 3, 4, 5]
    search_for = 6

    for item in items:
        if item == search_for:
            print(f"Found {search_for}!")
            break
    else:
        print(f"{search_for} not found in the list!") # "6 not found in the list!" is printed
    ```

    In the above example, since `6` is not in the list, the `else` block will be executed, and it will print "6 not found in the list!".
  - **`while` Loop with `else`:**

    Let's use a similar example, but with a `while` loop. Here, we'll be consuming the list while searching:

    ```python
    items = [1, 2, 3, 4, 5]
    search_for = 5

    while items:
        item = items.pop()
        if item == search_for:
            print(f"Found {search_for}!")
            break
    else:
        print(f"{search_for} not found in the list!") # Nothing is printed
    ```

    Since `5` is in the list, the `else` block will not be executed after the `while` loop finishes, and nothing will be printed.

- **Iterating through Dictionaries/Hashes**:
  - **Perl**: To iterate through a hash, you would typically use `each` or `keys` and `values` functions.
    ```perl
    while (my ($key, $value) = each %hash) {
        print "$key => $value\n";
    }
    ```
  - **Python**: You can iterate through dictionary keys, values, or key-value pairs using the `keys()`, `values()`, and `items()` methods, respectively.
    ```python
    for key, value in dictionary.items():
        print(f"{key} => {value}")
    ```
