# 📘 Python Notes (Complete)

---

## 🔹 1. Introduction to Python

- **High-level**, **interpreted**, **general-purpose** programming language.
    
- **Easy syntax**, suitable for beginners.
    
- Created by **Guido van Rossum**, first released in **1991**.
    

### Features

- Easy to learn
    
- Interpreted
    
- Dynamically typed
    
- Object-Oriented
    
- Rich standard library
    
- Platform-independent
    

---

## 🔹 2. Python Basics

### Variables and Data Types

python



`x = 10           # int name = "Alice"   # str pi = 3.14        # float is_valid = True  # bool`

### Type Casting

python



`int("10")      # 10 float("3.14")  # 3.14 str(100)       # "100"`

### Input/Output

python



`name = input("Enter your name: ") print("Hello", name)`

---

## 🔹 3. Operators

### Arithmetic

python



`+, -, *, /, %, // (floor division), ** (power)`

### Comparison

python



`==, !=, >, <, >=, <=`

### Logical

python



`and, or, not`

### Assignment

python



`=, +=, -=, *=, etc.`

---

## 🔹 4. Conditional Statements

python



`if x > 0:     print("Positive") elif x == 0:     print("Zero") else:     print("Negative")`

---

## 🔹 5. Loops

### While Loop

python



`i = 1 while i <= 5:     print(i)     i += 1`

### For Loop

python



`for i in range(5):     print(i)`

### Loop Control

python



`break, continue, pass`

---

## 🔹 6. Functions

python



`def greet(name):     print("Hello", name)  greet("Alice")`

### Return Statement

python



`def add(a, b):     return a + b`

---

## 🔹 7. Data Structures

### List

python



`fruits = ["apple", "banana"] fruits.append("mango")`

### Tuple

python



`coords = (10, 20)`

### Set

python



`s = {1, 2, 3} s.add(4)`

### Dictionary

python



`student = {"name": "John", "age": 21} print(student["name"])`

---

## 🔹 8. String Handling

python



`s = "Python" s.upper() s.lower() s[0] len(s) "th" in s`

---

## 🔹 9. File Handling

`# Write to a file with open("file.txt", "w") as f:     f.write("Hello")  # Read from a file with open("file.txt", "r") as f:     print(f.read())`

---

## 🔹 10. Exception Handling

`try:     x = 1 / 0 except ZeroDivisionError:     print("Cannot divide by zero") finally:     print("Always runs")`

---

## 🔹 11. Object-Oriented Programming (OOP)

### Class and Object

`class Person:     def __init__(self, name):         self.name = name      def greet(self):         print("Hello", self.name)  p = Person("Alice") p.greet()`

### Concepts

- Inheritance
    
- Encapsulation
    
- Polymorphism
    

---

## 🔹 12. Modules and Packages

### Importing

`import math print(math.sqrt(16))`

### Creating a Module


`# mymodule.py def add(a, b):     return a + b`

---

## 🔹 13. Lambda Functions


`add = lambda x, y: x + y print(add(5, 3))`

---

## 🔹 14. List Comprehensions

`squares = [x*x for x in range(10)]`

---

## 🔹 15. Useful Built-in Functions


`len(), max(), min(), sum(), sorted(), type()`

---

## 🔹 16. Python Libraries

- **NumPy** – for numerical computations
    
- **Pandas** – for data manipulation
    
- **Matplotlib** – for data visualization
    
- **Tkinter** – for GUI
    
- **Requests** – for HTTP requests
    
- **Flask / Django** – for web development
    

---

## 🔹 17. Virtual Environments

`python -m venv env source env/bin/activate  # Linux/macOS env\Scripts\activate     # Windows`

---

## 🔹 18. Pip Package Manager

`pip install package_name`

---

## 🔹 19. Decorators

`def decorator(func):     def wrapper():         print("Before")         func()         print("After")     return wrapper  @decorator def say_hello():     print("Hello")  say_hello()`

---

## 🔹 20. Generators

`def gen():     yield 1     yield 2     yield 3  for i in gen():     print(i)`

[[index]]
