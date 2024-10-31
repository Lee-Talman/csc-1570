# CSC1570 Week 10/11

### Compatibility Note
This report was written in Markdown format, and is best read in a markdown-compatible reader such as VS Code. Code blocks in this report use syntax highlighting for Python, which is available in nearly all common Markdown readers.

## Question 1

This homework is about [functions](https://docs.python.org/3/glossary.html#term-function). Question 1 asks us to describe what is happening in the following Python module:
```py
def main():
    function1()
    function2()

def function1():
    print("Inside Function1")

def function2():
    x = 3
    y = 2
    z = x * y
    print("Z = ", z)

main()
```
The best way to describe what is going on is by examining this code from the perspective of the Python [interpreter](https://docs.python.org/3/tutorial/interpreter.html), i.e. the program that is reading this code. This discussion gets more complicated when examining this code from the perspective of the Python compiler or the virtual machine, so we'll skip over that for now.

The Python interpreter reads this block of code from top-to-bottom. First, it finds three `def` keywords, indicating that the interpreter should remember their *def*-initions. In Python, however, the interpreter simply acknowledges that these definitions exist, but does not execute the code nested within them. These function definitions are stored in what's called the global [namespace](https://docs.python.org/3/glossary.html#term-namespace).

### The Global Namespace

The final line of the module, `main()`, is a *function call*, indicated by the `()` following `main`. This tells the Python interpreter to look for the `main` function in the global namespace and execute any code found at that location in your computer's memory. By utilizing a concept called [reflection](https://en.wikipedia.org/wiki/Reflective_programming), we can look at a [dictionary](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) of these function definitions (and other things) stored in our global namespace to confirm what's happening here. 

Python has a built-in function called `globals()` that will give us this information, which we can print to console output:

```py
# note that we're calling the *function* globals, so we need globals(), not globals
print(globals())
```

...will produce the following output in your terminal:
```bash
{
    '__name__': '__main__', 
    '__doc__': None, 
    '__package__': None, 
    '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x00000277329ABC80>, 
    '__spec__': None, 
    '__annotations__': {}, 
    '__builtins__': <module 'builtins' (built-in)>, 
    '__file__': 'c:\\Users\\MyName\\Documents\\CSC-1570\\week-10-11.py', 
    '__cached__': None, 
    'main': <function main at 0x000002773299A3E0>,
    'function1': <function function1 at 0x0000027732C3D120>, 
    'function2': <function function2 at 0x0000027732C3DE40>
}
```
Check out our last three `{ key : value }` pairs. Those are the *names of our functions* as keys, with their *location in memory* as values. Their locations are stored as 16-character [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) values representing 8 bytes, or 64 bits of computer memory. Ever notice how some operating systems and software still give you the option of 32-bit or 64-bit versions? This is where that distinction becomes important. We're printing this from a 64-bit operating system, so the memory values are 16 characters long.

### Executing the `main()` function

Let's rewrite the previous code block to contain only the "executing" code:

```py
main()
```

This looks a lot less intimidating, but unfortunately we're not done yet. We have defined three *custom* function definitions in the preceeding code - `"main"`, `"function1"`, and `"function2"`. The interpreter will make these replacements by itself, but let's go ahead and do it manually for practice:

```py
function1()
function2()
```

This code will execute `function1()` and then `function2()`. Thankfully, we've defined both of these functions previously. Let's start by replacing `function1()` with the definition from above: 

```py
print("Inside Function1")
function2()
```
From the perspective of the interpreter, we're going to execute a simple `print()` statement, and print out the words `"Inside Function1"`. Then, we'll move on to whatever `function2()` does. Let's replace `function2()` the same way:

```py
print("Inside Function1")
x = 3
y = 2
z = x * y
print("Z = ", z)
```
So now, we can look at this code in the same order that the interpreter is reading it. We print out the words `"Inside Function1"`, then define a variable `z = 3 * 2`, and print `"Z = "` followed by the value of `z` on the same line. 
```
Inside Function1
Z =  6
```

> **Note**: there's a space at the end of `"Z = "`, and the comma operator in `print()` adds a space by default, so the final line should have two spaces after the `=`.

At first, this seems a lot easier to understand than how we wrote the program earlier. Why bother with all of the boilerplate code when we can just write out the program linearly, as we've done in the past?

There are two problems with this approach:
1. **Reusing code**: we can't just write out all of these functions by hand every time we need to call them. Some of these functions can be incredibly large, and it makes the code difficult to read, maintain, and bloats the file such that it is difficult to store or send over the internet.

2. **Pass-by-object-reference**: Python utilizes an argument-passing model that is neither strictly "pass by value", nor "pass by reference", but rather switches between them based on the argument passed. Therefore, executing a block of code *inside* our `main()` function may return something different than packaging that code into a separate function and calling it *from* `main()`. If none of that makes sense to you right now, don't worry, because we'll work through an illustrative example of this concept in Question 2.