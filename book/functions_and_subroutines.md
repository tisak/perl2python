# Functions and Subroutines

## Defining and calling subroutines in PERL vs. functions in Python

Both Python and Perl allow for defining reusable blocks of code in the form of functions (in Python) and subroutines (in Perl). Let's look at the differences and similarities in their definitions, call syntax, and behaviors.

### Defining and Calling:
- **Perl**: 
  - Define a subroutine with the `sub` keyword.
    ```perl
    sub subroutine_name {
        # body
    }
    ```
  - Call a subroutine:
    ```perl
    $result = subroutine_name(args);
    ```
    
- **Python**:
  - Define a function with the `def` keyword.
    ```python
    def function_name(parameters):
        # body
    ```
  - Call a function:
    ```python
    result = function_name(args)
    ```

### Parameters and Arguments:
- **Perl**:
  - Positional parameters are available through the special array `@_`:
    ```perl
    sub greet {
        my ($name, $age) = @_;
        print "My name is $name and I'm $age years old.\n";
    }
    ```
  - Simulating named parameters using a hash:
    ```perl
    sub greet {
        my %args = @_;
        print "My name is $args{name} and I'm $args{age} years old.\n";
    }
    greet(name => "Alice", age => 30);
    ```

- **Python**:
  - Positional and default parameters:
    ```python
    def greet(name, age=30):
        print(f"My name is {name} and I'm {age} years old.")
    ```
  - Arbitrary number of arguments using `*args`:
    ```python
    def print_args(*args):
        for arg in args:
            print(arg)
    ```

### Returning Values:
- **Perl**: 
  - Return values using the `return` keyword.
    ```perl
    sub add {
        my ($a, $b) = @_;
        return $a + $b;
    }

    $result = add(2,3);
    print $result;
    ```
  - In Perl, a subroutine returns the value of the last statement by default if there's no `return`.
  
- **Python**:
  - Use the `return` statement.
    ```python
    def add(a, b):
        return a + b
    
    result = add(2,3)
    print(result)
    ```
  - Functions can return multiple values as a tuple.
    ```python
    def demo():
        return 1, "hello", 3.14
    
    values = demo()
    print(values)  # This will print the tuple
    ```

### Context in Perl:
- Perl subroutines can be aware of the context in which they're called (scalar vs. list) using `wantarray` and can adapt their behavior and return value accordingly:
  ```perl
  sub context_demo {
      return wantarray ? ('a', 'b', 'c') : 'abc';
  }

  my @list = context_demo();  # @list gets ('a', 'b', 'c')
  my $scalar = context_demo(); # $scalar gets 'abc'
  ```

While the general idea of functions and subroutines in Python and Perl is similar, there are nuances in their behavior, parameter handling, and context understanding. When transitioning from Perl to Python (or vice versa), it's essential to keep these differences in mind

## Argument passing by value vs. reference

In both Perl and Python, the way arguments are passed to functions/subroutines depends on what those arguments are and how they are handled. Here's a comparative analysis:

### Perl

1. **By Value**:
   - By default, arguments are passed to Perl subroutines by value. This means the subroutine receives a copy of the arguments, not a reference to the original variables.
   ```perl
   sub modify {
       my ($val) = @_;
       $val = 100;
   }
   
   my $x = 5;
   modify($x);
   print $x;  # This will still print 5.
   ```

2. **By Reference**:
   - To pass by reference in Perl, you can use references, which are essentially pointers to the original data.
   ```perl
   sub modify_ref {
       my ($ref_to_val) = @_;
       $$ref_to_val = 100;
   }
   
   my $x = 5;
   modify_ref(\$x);
   print $x;  # This will print 100.
   ```

### Python

1. **Immutable Data Types (Pass by Value)**:
   - Immutable data types like integers, floats, strings, and tuples are passed by value, i.e., a copy is passed.
   - When we say that an "immutable object is passed like a copy," what we mean is that any operations inside the function that would modify the object will actually create a new object in memory, and the local reference inside the function will then point to this new object.
   - The original object remains unchanged outside the function, and the reference outside the function still points to that original object.
   - Let's break it down with an example:
   ```python
   def modify(val):
       print(f"Inside before modification: {id(val)}")
       val += " World"
       print(f"Inside after modification: {id(val)}")
   x = "Hello"
   print(f"Outside before function call: {id(x)}")
   modify(x)
   print(f"Outside after function call: {id(x)}")
   print(x)
   ```
   Output:
   ```
   Outside before function call: [Memory Address 1]
   Inside before modification: [Memory Address 1]
   Inside after modification: [Memory Address 2]
   Outside after function call: [Memory Address 1]
   Hello
   ```

2. **Mutable Data Types (Pass by Reference)**:
   - Mutable data types like lists, sets, and dictionaries are passed by reference. This means changes made inside the function reflect in the original data.
   ```python
   def modify_list(lst):
       lst[0] = 100
   
   my_list = [5, 6, 7]
   modify_list(my_list)
   print(my_list)  # This will print [100, 6, 7].
   ```

It's crucial to note that while it seems like Python passes mutable data types by reference, what's actually happening is a bit subtler. Python passes the reference (i.e., the memory address of the object) by value. So while you can modify the object the reference points to, you cannot change the reference itself to point to a new object.

In essence, while the distinction between pass-by-value and pass-by-reference in Perl is clear and explicit (using references for the latter), Python's behavior is consistent—it always passes the reference by value—but the effect you see depends on the mutability of the object being referenced.

## Anonymous subroutines in PERL vs. lambda functions in Python

Anonymous subroutines and lambda functions serve similar purposes in Perl and Python, respectively: they allow you to define unnamed, inline functions for quick, simple operations. Let's delve into their similarities and differences:

### Anonymous Subroutines in Perl:

**Definition**: In Perl, you can create a subroutine without giving it a name. This is termed an anonymous subroutine.

**Syntax**:
```perl
my $func = sub {
    # Body of subroutine
};
```

**Usage**:
- Often used with functions like `map`, `grep`, etc.
- Can be more elaborate than Python's lambda functions, supporting multiple statements.

**Example**:
```perl
my $squared = sub {
    my $x = shift;
    return $x * $x;
};

print $squared->(4);  # Outputs: 16
```

### Lambda Functions in Python:

**Definition**: Python's `lambda` function allows you to create small, unnamed functions, typically for short, simple operations.

**Syntax**:
```python
lambda arguments: expression
```

**Usage**:
- Can only have a single expression, not multiple statements.
- Commonly used with functions like `sorted`, `filter`, `map`, etc.

**Example**:
```python
squared = lambda x: x * x

print(squared(4))  # Outputs: 16
```

### Key Differences:

1. **Complexity**: 
   - Perl's anonymous subroutines can contain multiple lines and are more versatile than Python's lambda functions.
   - Python's lambda functions are constrained to single expressions.

2. **Syntax**:
   - Perl uses the `sub` keyword without a name to define an anonymous subroutine.
   - Python uses the `lambda` keyword.

3. **Calling**:
   - In Perl, you invoke the anonymous subroutine using the arrow `->` operator.
   - In Python, you simply use the lambda function's variable name (or use it directly if not assigned to a variable).

4. **Common Use**:
   - Both are frequently employed in functional programming constructs, such as mapping and filtering.

Overall, while both Perl's anonymous subroutines and Python's lambda functions offer a way to define inline, unnamed functions, Perl's version is more flexible in terms of the complexity it can handle. On the other hand, Python's lambda functions are designed for brevity and simple tasks.

## Tables of functions

Python allows you to store standard funcions in data structures, for example Python allows you to create a dictionary where keys map to functions, including lambda functions (which are analogous to Perl's anonymous subroutines defined in hash map). You can then call them based on a parameter used as a key to the dictionary. You don't even need to use `eval`, as you might in Perl, because Python dictionaries provide direct access to the functions they store.

Here's a simple example to illustrate this:

```python
def greet():
    return "Hello!"

def farewell():
    return "Goodbye!"

# Dictionary with string keys and function values
func_dict = {
    "hello": greet,
    "goodbye": farewell,
    "square": lambda x: x * x  # A lambda function as a value
}

# Call a function based on a parameter/key
key = "hello"
if key in func_dict:
    result = func_dict[key]()
    print(result)  # Outputs: Hello!

# Using the lambda function
print(func_dict["square"](4))  # Outputs: 16
```

In the example above:
1. We have two named functions: `greet` and `farewell`.
2. We create a dictionary `func_dict` where keys like "hello" and "goodbye" map to these functions. We also add a lambda function with the key "square".
3. To call a function based on a key, you simply access the function via the dictionary and then call it. No need for `eval`.

This pattern can be very powerful in Python for creating lookup tables of functions, plugins, command dispatchers, and more. It's a native, more direct and safer approach than using `eval` in Perl.
