# CSC1570 Week 10/11

### Compatibility Note
This report was written in Markdown format, and is best read in a markdown-compatible reader such as VS Code. Code blocks in this report use syntax highlighting for Python, which is available in nearly all common Markdown readers.

## Question 1

Question 1 asks us to describe what is happening in the following Python source code:
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
The best way to describe what is going on is by examining this code from the perspective of the Python interpreter, i.e. the program that is reading this code. This discussion gets more complicated when examining this code from the perspective of the Python compiler or the virtual machine, so we'll skip over that for now.

The Python interpreter reads this block of code from top-to-bottom. First, it finds three `def` keywords, indicating that the interpreter should remember their *def*-initions. In Python, however, the interpreter simply acknowledges that these definitions exist, but does not execute the code nested within them. These function definitions are stored in what's called *global memory*. We'll get to that in a moment.

The final line, `main()`, is a *function call*, as indicated by the `()` following `main`. This tells the Python interpreter to look for the string `"main"` in `globals`. Python has many of these function definitions built-in by default, such as `"print"`, `"input"`, *etc*. The interpreter is handling these lookups automatically (*e.g.* looking up the code associated with `"main"` from the statement `main()`). A human can do the same thing as the interpreter by utilizing the concept of *reflection*. 

Reflection simply forces a computer program to look inward (as in looking at a mirror, hence "reflection"). In Python, we can force the interpreter to "reflect" and look for our function stored somewhere in its *global memory*. In order to do that, we first need to ask the interpreter to return *everything* in global memory. Python stores objects in global memory in the form of key : value pairs, and we can use Python's built-in `globals()` function to combine all of those key-value pairs into a single dictionary that we (both the human and the interpreter) can search through:

```py
# note that we're calling the *function* globals, so we need
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
Check out our last three key : value pairs. Those are the *names of our functions* as keys, with their *location in memory* as values. Their locations are stored as 16-character *hexadecimal* values representing 8 bytes, or 64 bits of computer memory. Ever notice how some operating systems and software still give you the option of 32-bit or 64-bit versions? This is where that distinction becomes important.

This is actually the only code that will be executed by the interpreter, at least at first. Let's rewrite the previous code block to contain only the "executing" code:

```py
main()
```

This looks a lot less intimidating, but unfortunately we're not done yet. We have defined three *custom* function definitions in the preceeding code - `"main"`, `"function1"`, and `"function2"`. The interpreter then uses the function's definition, `"main"`, to look up the code we've declared. Let's re-write this block of code as the interpreter is now executing it:


```py
function1()
function2()
```

So this code will simply execute `function1()` and `function2()`. Thankfully, both of those are functions we've previously defined, and we can perform the same substitution we performed previously. Let's start by replacing `function1()` with the definition we declared already:

```py
print("Inside Function1")
function2()
```
So from the perspective of the interpreter, we're going to execute a simple `print()` statement, and print out the words `"Inside Function1"`. Then, we'll move on to whatever `function2()` does. Let's replace `function2()` the same way:

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

Note that there's a space at the end of `"Z = "`, and the comma operator in `print()` adds a space by default, so the final line should have two spaces after the `=`.

At first, this seems a lot easier to understand than how we wrote the program earlier. Why bother with all of the boilerplate code when we can just write out the program linearly, as we've done in the past?

There are two problems with this approach. The first one is code reuse - we can't just write out all of these functions by hand every time we need to call them. Some of these functions can be incredibly large, and it makes the code difficult to read, maintain, and bloats the file such that it is difficult to store or send over the internet.

The second problem is the concept of "pass-by-value".

## Question 2
```py
def main():
    a = 3
    b = 7
    function1(a)
    print("After call to function1, A is: ", a)
    b = function2(b)
    print("After call to function2, B is: ", b)

def function1(a):   
    print("A is: ",a)
    a = a + 2
    print("A is now: ", a)
    a = a + 5

def function2(b):
    print("B is: ",b)
    b = b * 4
    print("B is now: ", b)
    return(b) 

main()

```
After typing in the code below and using the approach we established in Question 1, we can re-write this code block to show the executing code in order:

```py
def main():
    a = 3
    b = 7
    print("A is: ",a)
    
    a = a + 2
    print("A is now: ", a)
    a = a + 5
    
    print("After call to function1, A is: ", a)
    
    print("B is: ",b)
    b = b * 4
    print("B is now: ", b)
    return(b)
    
    print("After call to function2, B is: ", b)

main()

```
