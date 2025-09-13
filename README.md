Hangman Game in MIPS Assembly
📌 Overview

This project is a Hangman Game implemented in MIPS Assembly language.
The program randomly selects a word from a predefined word bank, and the player must guess the word letter by letter before the hangman figure is completely drawn.

It demonstrates low-level programming concepts such as:

Working with strings and arrays in memory

Input/output using MIPS system calls

Loops, conditions, and branching

Random number generation

State management (tracking correct/wrong guesses)

🎮 Game Rules

The program randomly selects a 5-letter word from the word bank.

The word is displayed as underscores (_) to represent hidden letters.

The player guesses one letter per turn.

If the letter is correct:

It is revealed in the word display.

A "Correct!" message is shown.

If the letter is incorrect:

A part of the hangman is drawn.

A "Wrong guess." message is shown.

The player wins if all letters are guessed before 8 wrong attempts.

The player loses if the hangman is fully drawn.

🛠 Features

Word Bank of 20 words (e.g., apple, mouse, debug, zebra, etc.).

Random word selection at the start of each game.

Step-by-step hangman drawing after wrong guesses.

Clear win/lose conditions with appropriate messages.

Clean word display updating in real-time.

▶️ How to Run

Open the project in MARS or SPIM.

Assemble the program (Assemble button in MARS).

Run the program (Go button).

Follow the on-screen instructions to play.

📂 File Structure
hangman_game.asm   # Main MIPS Assembly source code
README.md          # Project documentation

📝 Example Gameplay
 _______
|       |
|       (_)
|        |
|       \|/
|        |
|       / \
|_______
Wrong guesses: 3 / 8

_ _ _ _ _

Guess a letter: a
Correct!
