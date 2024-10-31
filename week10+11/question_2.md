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
