---
tags:
  - python
  - while-loop
  - conditional-statement
  - python-list
  - matrices
  - user-input-handling
  - random
  - function
  - time
  - function-calls
  - boolean
status: In Progress
topic: Python CLI Game
environment: Python
created: 2026-04-25
---

## Overview

Match Fiesta is an English-Spanish word matching game in Python by Microsoft Visual Studio Code for Education. (hyperlink this to the website). Along the way, the original script needs to be upgraded with new in-game features:

- Challenge 1: Expand the Vocabulary
- Challenge 2: Add a Scoring System
- Challenge 3: Add a Game Timer
- Challenge 4: Create a 'Lives' Feature
- Challenge 5: Create a 'Hint' Feature

## Original Script
```python
# Step 1: Import the random module to use for shuffling lists
import random

  # Step 2: Create a function to generate randomized matrices from English and Spanish word lists.

def generate_random_matrix(rows, cols, words1, words2):

    # Calculate the total number of words needed for the matrices
    total_words = rows * cols
    if total_words > len(words1) or total_words > len(words2):
        raise ValueError("Matrix size exceeds the number of available words.")
    paired_words = list(zip(words1, words2))
    random.shuffle(paired_words)
    english_words, spanish_words = zip(*paired_words)

    # Shuffle Spanish words to ensure they don't line up
    spanish_words = list(spanish_words)
    random.shuffle(spanish_words)
    matrix1 = []
    matrix2 = []

    for i in range(rows):
        row1 = english_words[i * cols: (i + 1) * cols]
        row2 = spanish_words[i * cols: (i + 1) * cols]
        matrix1.append(list(row1))
        matrix2.append(list(row2))
    return matrix1, matrix2

# Step 3: Display the English and Spanish word matrices side by side

  def display_matrices(matrix1, matrix2):
    for i in range(len(matrix1)):
        for j in range(len(matrix1[i])):

            # Print each word from both matrices in the same row
            word1 = matrix1[i][j] if j < len(matrix1[i]) else ""
            word2 = matrix2[i][j] if j < len(matrix2[i]) else ""

            # Align English words to the left with 20 spaces for readability
            print(f"{word1.ljust(20)} {word2}")
    print()  # Print a new line after each row

# Step 4: Define the match_words function to handle user input and check if the entered word pair is correct

def match_words(matrix1, matrix2, word_pairs):

    user_input = input("Enter a pair (English Spanish) or type 'quit' to exit or 'restart' to restart the game: ").strip()

    if user_input.lower() == 'quit':
        print("Game ended.")
        return None, None

    if user_input.lower() == 'restart':
        main()
        return None, None

    user_input = user_input.split()

    if len(user_input) != 2:
        print("Error: Please enter exactly two words.")
        return matrix1, matrix2
    english_word, spanish_word = user_input

    # Check if the pair matches any in the list of correct word pairs

    if (english_word, spanish_word) in word_pairs:
        print("Correct match!")

        # Remove the matched words from the matrices
        matrix1, matrix2 = clear_matched_words(matrix1, matrix2, english_word, spanish_word)

        # Remove the matched pair from the word_pairs list
        word_pairs.remove((english_word, spanish_word))

    else:
        print("Incorrect match. Try again.")
    return matrix1, matrix2

# Step 5: Define the clear_matched_words function to remove matched words from the matrices

def clear_matched_words(matrix1, matrix2, english_word, spanish_word):
    for i in range(len(matrix1)):
        for j in range(len(matrix1[i])):
            if matrix1[i][j] == english_word:
                matrix1[i][j] = ""  # Clear the matched English word
            if matrix2[i][j] == spanish_word:
                matrix2[i][j] = ""  # Clear the matched Spanish word

    return matrix1, matrix2

# Step 6: Define the main function to start and manage the word matching game

def main():

    # Define lists of English and Spanish words
    english_words = ['apple', 'strawberry', 'orange', 'grape', 'pear', 'peach']
    spanish_words = ['manzana', 'fresa', 'naranja', 'uva', 'pera', 'durazno']

    # Set the number of rows and columns for the matrices
    rows, cols = 3, 2

    # Generate randomized matrices from the word lists
    matrix1, matrix2 = generate_random_matrix(rows, cols, english_words, spanish_words)

    # Display the matrices side by side
    display_matrices(matrix1, matrix2)

    # Create a list of correct word pairs for checking matches
    word_pairs = list(zip(english_words, spanish_words))

     # Start the game loop
    while word_pairs:

        # Handle user input and update the matrices
        matrix1, matrix2 = match_words(matrix1, matrix2, word_pairs)

        if matrix1 is None:  # This happens if the user quits or restarts the game
            break

        # Display updated matrices
        display_matrices(matrix1, matrix2)

    # Congratulate the player
    if matrix1 is not None:  # Only if the game was not quit
        print("Congratulations! You've matched all the words!") 

# Start the game when launched
main()
```

---

## Challenge 1:  Expand the Vocabulary

Expand the existing word-matching game by adding more English-Spanish word pairs and adjusting the matrix size to accommodate the new vocabulary. 

### Objective

- Increase the number of word pairs (10 more English-Spanish word pairs )
- Adjust the matrix size to match the new data (4x4)
- Keep the display aligned and readable

### Step 1: Expanded Word Lists

Added 10 more English–Spanish pairs to improve gameplay variety at `def main()` block:

![[Pasted image 20260425130217.png|697]]
### Step 2: Adjust Matrix Size

Since there are 16 pairs after the addition of English-Spanish pairs, the `rows` and `cols` are adjusted to 4 x 4.
![[Pasted image 20260425130631.png]]

### Verify the Result of Challenge 1

- Script runs without error
- Word lists have **equal length**
- Matrix size match total elements -`rows * cols = len(word_list)`
- More words increase difficulty and replay value

![[Pasted image 20260425131415.png]]

---
## Challenge 2:  Add a Scoring System

Adds a scoring system to the game, rewarding correct matches and penalising incorrect ones. At the end of the game, or when user quits the game, the final score is displayed.

### Objective

- Track player performance using a `score` variable
- Update score based on correct (+10 points) and incorrect (-5 points) matches
- Display score after each attempt and at the end

### Step 1: Create a `score` Variable

Create `score` = 0 inside the `main()` body
![[Pasted image 20260425140849.png]]

### Step 2: Update the `score` 

Pass the `score` variables inside the `match_words` function as a new parameter, and `return` the updated `score` value.  

![[Pasted image 20260425141955.png]]

Inside the function's logic, add 10 points for each correct match and subtract 5 points for each incorrect match. Also, add a`print` statement for the `score` after user's feedback and update early `return`.  
 
![[Pasted image 20260425142412.png]]

### Step 3: Update `main()` loop

Add `score` to the function call in `while` loop, and `print` formatted-strings (`f-string`) statement for the end message to display the final `score` output. 
![[Pasted image 20260425142919.png]]

### Verify the Result of Challenge 2

- Script runs without error
- The game now tracks performance using a scoring system on each correct and incorrect answer after each user's attempt
- Displays a final score at the end

![[Pasted image 20260425140311.png]]

![[Pasted image 20260425140510.png]]

---

## Challenge 3:  Add a Game Timer

Add a timer to the game, increasing the difficulty by limiting the amount of time players have to complete the game. This task involves using Python's `time` module to track elapsed time and control the game’s flow based on real-time data. 

### Objective

- Limit how long the player can play
- End the game when time runs out
- Display progress and bonus score

### Step 1: Import the `time` Module

This module provides functions to track elapsed `time`.

![[Pasted image 20260425153211.png]]

### Step 2: Set a Time Limit

Inside `main()`, set a new variable `time_limit = 60` seconds.

![[Pasted image 20260425153417.png]]

### Step 3: Track Time

Capture the `start time` before the game loop:

![[Pasted image 20260425153741.png]]

Create a logic to check for `elapsed_time` inside the loop:

![[Pasted image 20260425154536.png]]

### Step 4: Time's Up Message

Inside `main()`, Add a counter to track how many `matches_count` were completed.

![[Pasted image 20260425154840.png]]

Update inside `match_words()` to add counter+1 when correct:

![[Pasted image 20260425160848.png]]

Just like when updating `score` previously, since `matched_count` is also added into one of the `match_words` parameters, pass all `matched_count` variables and make sure all `return` have the same number of 4 values.

![[Pasted image 20260425161327.png]]
![[Pasted image 20260425161918.png]]

Inside the `loop`, add `matched_count` as an argument to call the `match_words` function. Also, `print` how many `matched_count` when the time is up. 

![[Pasted image 20260425161619.png]]

---

### Step 4: Adjust Scoring System with Bonus Score

The game adds bonus points for completing the game before the time runs out by modifying the logic:

- `if matrix1 is not None` -> if the game is not ended because user chooses quit or restart, `and`
- `not word_pairs` -> all the word pairs are matched / word list is empty,
- Create a variable called `remaining_time` that calculates:
	- `max(0,...)` -> ensures that the time is not a negative value
	- `time_limit - elapsed_time` -> calculate the `remaining_time` 
	- Create another variable called `bonus` that stores the value of `remaining_time` in `int` value
	- Add the `score` with `bonus` 
- Print the `bonus` score to the final end message

![[Pasted image 20260425171439.png]]

### Verify the Result of Challenge 3

- Script runs without error.
- The game now has a time limit.

![[Pasted image 20260425160734.png]]

- Bonus score is calculated into the final score if player can complete all word pairs before the time ends.

![[Pasted image 20260425164259.png]]

---

## Challenge 4:  Create a 'Lives' Feature

The lives system will introduce a new layer of difficulty, limiting the number of incorrect attempts a player can make before the game ends. 

### Objective

- Limit incorrect attempts using a `lives` variable
- End the game when lives reach 0
- Display remaining lives after each attempt

### Step 1: Create a Lives Variable

Inside `main()`:

![[Pasted image 20260425173812.png]]

### Step 2: Update Function Parameters

Pass `lives` into `match_words()` and make sure all `return` values are updated with `lives`. 

![[Pasted image 20260425174343.png]]
![[Pasted image 20260425174440.png]]

### Step 3: Decrease Lives of Incorrect Match

Inside `match_words()` function, add `lives -= 1` inside the body of `else`:
![[Pasted image 20260425174747.png]]

### Step 4: Display Remaining Lives

![[Pasted image 20260425175054.png]]

### Step 5: End Games when Lives = 0

Add a new `if` condition in the body of `match_words()` function after updating lives:

![[Pasted image 20260425175602.png]]

### Step 6: Update Function Calls

Inside the `while` loop body, update `match_words` function calls with `lives`. 

![[Pasted image 20260425180110.png]]

### Verify the Result of Challenge 4

- Script runs without error.
- The game now has a failure condition.
- When player makes a wrong answer, lives reduces by 1. 

![[Pasted image 20260425180158.png]]

- When player makes wrong answer three times, lives becomes 0 and the game is over.

![[Pasted image 20260425180232.png]]

---

## Challenge 5:  Create a 'Hint' Feature

This challenge focuses on creating a hint system that allows players to request a hint if they get stuck. However, using a hint comes with a cost: points will be deducted from the player's score. 

### Objective

- Allow player to type `"hint"`
- Reveal one correct pair
- Deduct score for using a hint
- Continue the game normally

### Step 1: Add Hint Option

Inside `match_words()` function, update the string with `hint` keyword as an option.

![[Pasted image 20260425202543.png]]

### Step 2: Detect `hint` Input

Inside `match_words()` function, add an `if` condition before `user_input = user_input.split()`:

![[Pasted image 20260425203346.png]]

### Step 3: Deduct Points

Inside the `if` condition body, input `score -= 10` and `print` explanation text to user.

![[Pasted image 20260425203507.png]]

### Step 4: Reveal a Hint

Randomly select a pair from remaining `word_pairs` and `print` the selected `hint`.

- `random.choice(word_pairs)`  --> Calls a function from the `random` module that returns a **random element from a list** (`word_pairs`).
- `hint_pair = ...`  --> Assigns the selected tuple (pair of words) to the variable `hint_pair`.
- `english_word, spanish_word = hint_pair`  --> Uses **tuple unpacking** to assign each value in the tuple to separate variables.
- `print(f"...")`  --> Uses an **f-string** to format and display the hint with variable values inserted into the string.

![[Pasted image 20260425204932.png]]

### Step 5: Remove the Hint Pair

- `clear_matched_words(...)`  --> Calls a user-defined function to update both matrices by replacing matched words with empty strings.
- `matrix1, matrix2 = ...`  --> Uses **multiple assignment** to store the updated matrices returned by the function.
- `word_pairs.remove(hint_pair)`  --> Calls the list method `.remove()` to delete the revealed pair from the list, preventing it from being matched again.
- `matched_count += 1`  --> Increments the counter variable using an assignment operator.

![[Pasted image 20260425205230.png]]

### Step 6: Return and Continue Game

- `print(f"...")`  --> Uses **formatted string literals (f-strings)** to display updated values of variables.
- `return matrix1, matrix2, score, matched_count, lives`  --> Returns multiple values as a **tuple**, which will be unpacked in the calling function (`main()`).

![[Pasted image 20260425205555.png]]

### Verify the Result of Challenge 5

- Script runs without error.
- When player typed `hint`, the screen displays score deduction, selects word pair randomly from the list, and displays the current score deduction - 10. 
- The selected hint pair, *book =* *libro*, is removed from the list.
- The game continues as usual, prompting player to type the next word pair. 

![[Pasted image 20260425205807.png]]

---

## Final Script - Match Fiesta

```python
# Step 1: Import the random module to use for shuffling lists  
import random  
import time  
  
# Step 2: Create a function to generate randomized matrices from English and Spanish word lists  
def generate_random_matrix(rows, cols, words1, words2):  
  
    # Calculate the total number of words needed for the matrices  
    total_words = rows * cols  
    if total_words > len(words1) or total_words > len(words2):  
        raise ValueError("Matrix size exceeds the number of available words.")  
  
    paired_words = list(zip(words1, words2))  
    random.shuffle(paired_words)  
  
    english_words, spanish_words = zip(*paired_words)  
  
    # Shuffle Spanish words to ensure they don't line up  
    spanish_words = list(spanish_words)  
    random.shuffle(spanish_words)  
  
    matrix1 = []  
    matrix2 = []  
  
    for i in range(rows):  
        row1 = english_words[i * cols: (i + 1) * cols]  
        row2 = spanish_words[i * cols: (i + 1) * cols]  
        matrix1.append(list(row1))  
        matrix2.append(list(row2))  
  
    return matrix1, matrix2  
  
# Step 3: Display the English and Spanish word matrices side by side  
def display_matrices(matrix1, matrix2):  
    for i in range(len(matrix1)):  
        for j in range(len(matrix1[i])):  
  
            # Print each word from both matrices in the same row  
            word1 = matrix1[i][j] if j < len(matrix1[i]) else ""  
            word2 = matrix2[i][j] if j < len(matrix2[i]) else ""  
  
            # Align English words to the left with spacing for readability  
            print(f"{word1.ljust(20)} {word2}")  
        print()  # Print a new line after each row  
  
# Step 4: Handle user input and check if the entered word pair is correct  
def match_words(matrix1, matrix2, word_pairs, score, matched_count, lives):  
  
    user_input = input(  
        "Enter a pair (English Spanish) or type 'quit' to exit, 'hint', or 'restart' to restart the game: "  
    ).strip()  
  
    if user_input.lower() == 'quit':  
        print("Game ended.")  
        return None, None, score, matched_count, lives  
  
    if user_input.lower() == 'restart':  
        main()  
        return None, None, score, matched_count, lives  
  
    if user_input.lower() == 'hint':  
        score -= 10  
        print("Hint used. Score deduction: - 10 points.")  
  
        hint_pair = random.choice(word_pairs)  
        english_word, spanish_word = hint_pair  
  
        print(f"Hint: {english_word} = {spanish_word}")  
  
        matrix1, matrix2 = clear_matched_words(matrix1, matrix2, english_word, spanish_word)  
        word_pairs.remove(hint_pair)  
        matched_count += 1  
  
        print(f"Current score: {score}")  
        print(f"Lives remaining: {lives}")  
        return matrix1, matrix2, score, matched_count, lives  
  
    user_input = user_input.split()  
  
    if len(user_input) != 2:  
        print("Error: Please enter exactly two words.")  
        return matrix1, matrix2, score, matched_count, lives  
  
    english_word, spanish_word = user_input  
  
    # Check if the pair matches any in the list of correct word pairs  
    if (english_word, spanish_word) in word_pairs:  
        print("Correct match!")  
        score += 10  
        matched_count += 1  
  
        # Remove the matched words from the matrices  
        matrix1, matrix2 = clear_matched_words(  
            matrix1, matrix2, english_word, spanish_word  
        )  
  
        # Remove the matched pair from the word_pairs list  
        word_pairs.remove((english_word, spanish_word))  
  
    else:  
        print("Incorrect match. Try again.")  
        score -= 5  
        lives -= 1  
  
    print(f"Current score:{score}")  
    print(f"Current Lives:{lives}")  
  
    if lives == 0:  
        print("Game Over! You've run out of lives.")  
        return None, None, score, matched_count, lives  
  
    return matrix1, matrix2, score, matched_count, lives  
  
# Step 5: Remove matched words from the matrices  
def clear_matched_words(matrix1, matrix2, english_word, spanish_word):  
    for i in range(len(matrix1)):  
        for j in range(len(matrix1[i])):  
            if matrix1[i][j] == english_word:  
                matrix1[i][j] = ""  # Clear the matched English word  
            if matrix2[i][j] == spanish_word:  
                matrix2[i][j] = ""  # Clear the matched Spanish word  
  
    return matrix1, matrix2  
  
# Step 6: Main function to start and manage the game  
  
def main():  
    score = 0  
    matched_count = 0  
    time_limit = 100 # seconds  
    start_time = time.time()  
    lives = 3  
  
    # Define lists of English and Spanish words  
    english_words = [  
        'apple', 'strawberry', 'orange', 'grape', 'pear', 'peach',  
        'God', 'blue', 'red', 'table', 'cat', 'money',  
        'car', 'house', 'book', 'dog'  
    ]  
  
    spanish_words = [  
        'manzana', 'fresa', 'naranja', 'uva', 'pera', 'durazno',  
        'Dios', 'azul', 'roja', 'mesa', 'gato', 'dinero',  
        'coche', 'casa', 'libro', 'perro'  
    ]  
  
    # Set the number of rows and columns  
    rows, cols = 4, 4  
  
    # Generate randomized matrices  
    matrix1, matrix2 = generate_random_matrix(rows, cols, english_words, spanish_words)  
  
    # Display matrices  
    display_matrices(matrix1, matrix2)  
  
    # Create correct word pairs  
    word_pairs = list(zip(english_words, spanish_words))  
  
    # Game loop  
    while word_pairs:  
  
        elapsed_time = time.time() - start_time  
        if elapsed_time > time_limit:  
            print(f"Time's up! You matched {matched_count} words correctly.")  
            print(f"Your final score is: {score}")  
            break  
  
        matrix1, matrix2, score, matched_count, lives = match_words(matrix1, matrix2, word_pairs, score, matched_count, lives)  
  
        if matrix1 is None:  # User quit or restart  
            break  
  
        display_matrices(matrix1, matrix2)  
  
    # End message  
    # Check if the game ended normally (not quit) and all word pairs are matched    if matrix1 is not None and not word_pairs:  
        # Calculate remaining time (ensure it is not negative)  
        remaining_time = max(0, time_limit - elapsed_time)  
        bonus = int(remaining_time)  
        # add bonus point to score  
        score += bonus  
        print("Congratulations! You've matched all the words!")  
        print(f"Time Bonus for completing before time ends: {bonus}")  
        print(f"Your final score is: {score}")  
  
# Start the game  
main()
```