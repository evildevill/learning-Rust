# Rust Guessing Game

## What I Learned in Chapter 2

In Chapter 2, I built a simple "Guessing Game" in Rust. The game asks the player to guess a number between 1 and 100, then tells them whether their guess is too low, too high, or correct. Below is a detailed explanation of each step and the concepts I learned.

### 1. Setting Up a New Project with Cargo

**Cargo** is Rust's package manager and build system, which makes it easy to manage projects. When you create a new project with Cargo, it sets up a basic project structure for you.

- **Creating a New Project**: 
  ```bash
  cargo new guessing_game
  ```
  - This command creates a new directory named `guessing_game`.
  - Inside this directory, Cargo generates a `Cargo.toml` file and a `src` folder containing a `main.rs` file.
  - The `Cargo.toml` file holds metadata about your project, such as its name, version, and dependencies.
  - The `src` folder is where you’ll write your Rust code.

### 2. Writing the Main Function

Every Rust program starts execution from a function called `main`. It’s the entry point of the application, similar to the `main` function in languages like C or C++.

- **Writing `main.rs`:**
  ```rust
  fn main() {
      println!("Guess the number!");
  }
  ```
  - `fn` is the keyword used to declare a function.
  - `main` is the name of the function.
  - The parentheses, `()`, indicate there are no parameters.
  - The curly braces `{}` define the body of the function, where the code that runs is placed.
  - `println!` is a macro that prints text to the console. The `!` indicates that it’s a macro, not a regular function.

### 3. Using `let` to Create Variables

Variables in Rust are immutable by default, meaning once you assign a value, it cannot change. However, you can make a variable mutable with the `mut` keyword, allowing you to modify it later.

- **Creating a Mutable Variable**:
  ```rust
  let mut guess = String::new();
  ```
  - `let` declares a new variable.
  - `mut` allows the variable `guess` to be modified.
  - `String::new()` creates a new, empty string. This string will store the user’s input.

In Rust, the `::` syntax is called the **path separator** or **namespace separator**. It's used to navigate through different modules, types, or functions within the Rust code. When you use `::new`, you are accessing the `new` function in the context of a specific type or module.

For example:
- `std::io::Read` refers to the `Read` trait in the `std::io` module.
- `String::new()` calls the `new` associated function on the `String` type.

So, you can refer to the `::` as the **path separator** or **namespace separator** in this context.

### 4. Reading User Input

To get input from the user, I used the `std::io` module, which provides input and output functionalities. The `stdin` function reads input from the console.

- **Reading Input from the User**:
  ```rust
  use std::io;

  io::stdin().read_line(&mut guess).expect("Failed to read line");
  ```
  - `use std::io;` imports the `io` module from Rust’s standard library.
  - `io::stdin()` accesses the standard input.
  - `read_line(&mut guess)` reads what the user types and stores it in the `guess` variable.
  - The `&mut guess` syntax passes a mutable reference of `guess` to the function, allowing it to modify `guess` directly.
  - `expect("Failed to read line")` handles potential errors by printing a message if something goes wrong while reading the input.

### 5. Handling Potential Errors

Rust encourages handling errors to ensure your programs run smoothly. Instead of letting errors cause a crash, Rust allows you to manage them.

- **Handling Errors with `expect`**:
  ```rust
  .expect("Failed to read line");
  ```
  - The `expect` method is called on the result of `read_line`.
  - If `read_line` succeeds, `expect` does nothing.
  - If `read_line` fails, `expect` prints the provided error message and stops the program. This prevents the program from continuing with invalid data.

### 6. Converting Strings to Numbers

User input is received as a string, but to compare the input to the secret number, it must be converted into a number. This is done using the `trim` and `parse` methods.

- **Converting the Input**:
  ```rust
  let guess: u32 = guess.trim().parse().expect("Please type a number!");
  ```
  - `guess.trim()` removes any leading or trailing whitespace from the input, which is important because the `read_line` function adds a newline character at the end of the input.
  - `guess.parse()` attempts to convert the string into another type, in this case, a `u32` (an unsigned 32-bit integer).
  - If the string cannot be converted to a number, `expect("Please type a number!")` will display an error message.

### 7. Generating a Random Number

The game needs a secret number for the player to guess. This number is randomly generated using the `rand` crate, which provides random number generation functionality.

- **Adding `rand` to `Cargo.toml`**:
  ```toml
  [dependencies]
  rand = "0.8"
  ```
  - Adding this line in `Cargo.toml` tells Cargo to include the `rand` crate in the project.

- **Generating the Secret Number**:
  ```rust
  use rand::Rng;

  let secret_number = rand::thread_rng().gen_range(1..=100);
  ```
  - `use rand::Rng;` brings the `Rng` trait into scope, which defines methods for random number generation.
  - `rand::thread_rng()` creates a random number generator.
  - `gen_range(1..=100)` generates a random number between 1 and 100, inclusive.

### 8. Using `match` for Control Flow

The game uses a `match` statement to compare the player’s guess with the secret number. Based on the comparison, it provides feedback.

- **Comparing the Guess**:
  ```rust
  use std::cmp::Ordering;

  match guess.cmp(&secret_number) {
      Ordering::Less => println!("Too small!"),
      Ordering::Greater => println!("Too big!"),
      Ordering::Equal => {
          println!("You win!");
          break;
      }
  }
  ```
  - `use std::cmp::Ordering;` imports the `Ordering` enum, which has three variants: `Less`, `Greater`, and `Equal`.
  - `guess.cmp(&secret_number)` compares the guess to the secret number and returns an `Ordering` variant.
  - The `match` statement checks which `Ordering` was returned and executes the corresponding code block:
    - `Ordering::Less`: The guess is too small.
    - `Ordering::Greater`: The guess is too big.
    - `Ordering::Equal`: The guess is correct, and the game prints a victory message and exits the loop with `break`.

Here’s the completion of the section on looping until the player wins, based on the provided code:

### Looping Until the Player Wins

To keep the game running until the player guesses the correct number, a `loop` is used. This loop ensures the game continues to ask for input and provide feedback until the correct guess is made.

- **Creating a Loop**:
  ```rust
  loop {
      println!("Please input your guess.");
  
      let mut guess = String::new();
  
      io::stdin()
          .read_line(&mut guess)
          .expect("Failed to read line");
  
      let guess: u32 = match guess.trim().parse() {
          Ok(num) => num,
          Err(_) => continue,
      };
  
      println!("You guessed: {guess}");
  
      match guess.cmp(&secret_number) {
          Ordering::Less => println!("Too small!"),
          Ordering::Greater => println!("Too big!"),
          Ordering::Equal => {
              println!("You win!");
              break;
          }
      }
  }
  ```

  - **`loop {}`**: The loop here is an infinite loop, meaning it will keep running indefinitely until something inside it causes it to stop. In this case, the loop continues to run until the player guesses the correct number.
  
  - **Game Logic Inside the Loop**:
    - **Input Prompt**: Each time the loop iterates, the game prompts the player to input their guess.
    - **Reading and Parsing the Input**: The player’s input is read and then parsed into a number. If the input is invalid (e.g., not a number), the loop will simply skip to the next iteration and ask for input again, thanks to the `continue` statement in the `Err(_) => continue` match arm.
    - **Printing the Guess**: The guessed number is printed to the console for feedback.
    - **Comparing the Guess**: The `match` statement compares the player’s guess to the secret number:
      - If the guess is **too small** (`Ordering::Less`), the game informs the player and the loop continues.
      - If the guess is **too big** (`Ordering::Greater`), the game informs the player and the loop continues.
      - If the guess is **correct** (`Ordering::Equal`), the game prints a congratulatory message ("You win!") and the loop is exited using `break`.
  
  - **`break`**: When the player guesses the correct number, the loop ends, and the game finishes. The `break` statement is crucial as it stops the loop from running indefinitely once the game is won.

## Conclusion

In this chapter, I built a simple yet interactive guessing game that taught me several fundamental Rust concepts. I learned how to set up a project with Cargo, write and structure code, handle user input and errors, and implement basic control flow. This chapter provided a hands-on introduction to Rust’s syntax and its standard library.

- Author: [Waseem Akram](https://hackerwasii.com)
- GitHub: [evildevill](https://github.com/evildevill)
