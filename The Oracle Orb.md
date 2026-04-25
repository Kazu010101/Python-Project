---
tags:
  - python
  - random
  - while-loop
  - conditional-statement
  - python-list
status: Complete
topic: Python CLI Game
environment: Python
created: 2026-04-24
---

## Overview

The Oracle Orb is a command line game as part of Python Lesson by Microsoft Visual Studio Code for Education. (hyperlink this to the website)
The user asks a question and receives a random response.  
The program repeats using a `while` loop until the user chooses to stop.

>>*The "Oracle Orb" is a fortune telling game. The player asks a question, shakes the orb, and then looks at the dice to see the answer. The player can then ask another question and repeat the process. The player can ask as many questions as they like. The Oracle Orb is always right!*
---

## Objective

The goal of this project is to simplify the original implementation by:

- Reducing repetitive `if/elif` statements
- Using a `list` data structure
- Using built-in functions from the `random` module
- Improving readability and maintainability of the original script

---

## Original Implementation

### Key Characteristics

- Uses `random.randint()` to generate a number between 1 and 20
- Uses a long `if/elif` conditional chain to match each number to a response
- Logic is repetitive and harder to maintain due to long line

### Original Code

The original script uses twenty `if/elif` functions, each providing different `answer` to the user's input, regulated by the `random` module and calling of `.randint` function. 

```python
# Import random module to use the randint function
import random

print("Welcome to the Oracle Orb!")

play_again = "Y"

# While loop condition: calls the str.upper() method on play_again and compares it to "Y";
# loop continues executing as long as the boolean expression evaluates to True
while play_again.upper() == "Y":
    input("Please ask a question: ")
    print("That's a great question. Hmmm, let me think about this one...")

    answer = random.randint(1, 20)

    if answer == 1:
        print("It is certain.")
    elif answer == 2:
        print("It is decidedly so.")
    elif answer == 3:
        print("Without a doubt.")
    elif answer == 4:
        print("Yes definitely.")
    elif answer == 5:
        print("You may rely on it.")
    elif answer == 6:
        print("As I see it, yes.")
    elif answer == 7:
        print("Most likely.")
    elif answer == 8:
        print("Outlook good.")
    elif answer == 9:
        print("Yes.")
    elif answer == 10:
        print("Signs point to yes.")
    elif answer == 11:
        print("Reply hazy, try again.")
    elif answer == 12:
        print("Ask again later.")
    elif answer == 13:
        print("Better not tell you now.")
    elif answer == 14:
        print("Cannot predict now.")
    elif answer == 15:
        print("Concentrate and ask again.")
    elif answer == 16:
        print("Don't count on it.")
    elif answer == 17:
        print("My reply is no.")
    elif answer == 18:
        print("My sources say no.")
    elif answer == 19:
        print("Outlook not so good.")
    elif answer == 20:
        print("Very doubtful.")

    play_again = input("Would you like to ask another question? Y/N: ")

print("Thank you for consulting the Oracle Orb. Have a good day!")
```

---

## Refined Implementation

### Key Improvements

- Replaced `if/elif` chain with a `list`
- Used `random.choice()` to select a response
- Reduced code length and improved readability
- Easier to update or extend responses

### Refined Code

Instead of long `if/elif`, use a `list` to store the responses in a variable called `responses`.

Then, inside the `while` loop body, call the `choice()` function from the `random` module to return a random element from the `responses` `list`, then `print` it.
```python
import random  
  
responses = [  
    "It is certain.", "It is decidedly so.", "Without a doubt.",  
    "Yes definitely.", "You may rely on it.", "As I see it, yes.",  
    "Most likely.", "Outlook good.", "Yes.", "Signs point to yes.",  
    "Reply hazy, try again.", "Ask again later.", "Better not tell you now.",  
    "Cannot predict now.", "Concentrate and ask again.",  
    "Don't count on it.", "My reply is no.", "My sources say no.",  
    "Outlook not so good.", "Very doubtful."  
]  
  
print("Welcome to the Oracle Orb!")  
  
play_again = "Y"  
  
# While loop condition: evaluates to True while user input (converted to uppercase) is "Y"  
while play_again.upper() == "Y":  
    input("Please ask a question: ")  
    print("That's a great question. Hmmm, let me think about this one...")  
  
    # Function call: returns a random element from the responses list  
    print(random.choice(responses))  
    play_again = input("Would you like to ask another question? Y/N: ")  
  
print("Thank you for consulting the Oracle Orb. Have a good day!")
```

---
## Comparison

| Aspect          | Original             | Refined           |
| --------------- | -------------------- | ----------------- |
| Structure       | Long `if/elif` chain | Uses `list`       |
| Random Logic    | `random.randint()`   | `random.choice()` |
| Readability     | Lower                | Higher            |
| Maintainability | Harder to update     | Easier to update  |
| Code Length     | 59 lines             | 26 lines          |

---

## Key Learning Points

- A `list` can replace repetitive conditional logic
- `random.choice()` is more suitable than `random.randint()` for selecting items
- A `while` loop is useful for repeated user interaction
- Writing simpler code improves readability and maintainability

---
## Conclusion

This refactoring shows how Python data structures and built-in functions can simplify logic.  
The final version is shorter, clearer, and easier to maintain while keeping the same functionality.
