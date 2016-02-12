# Homework 4: Reverse Polish ~~Sausage~~ Notation Calculator

**Due 2016-02-17, 11:59pm.**

For questions, please post on Piazza (Penn students) or Google Groups (other).
Links on course homepage.

## Overview

Reverse Polish notation, which is [not exactly][rpn] the reverse of Polish
notation, is a convention for mathematical expressions for ~~nerds~~
~~hipsters~~ ~~enterprising young computer scientists~~ ~~bored students~~ you!
Implementing an RPN calculator in Rust will cover several standard library
modules and common patterns.

[rpn]: https://en.wikipedia.org/wiki/Reverse_Polish_notation#Explanation

Reverse Polish notation defines expressions with _postfix_ operators (`1 2 +`),
rather than the typical _infix_ expression form (`1 + 2`). Postfix form is very
easy to evaluate using a state machine with a stack data structure. When
the calculator encounters a numeric literal (`1`), it pushes the number onto the
stack. When an operator (`+`) is encountered, the machine pops one or more
numbers off the stack, evaluates the expression, then pushes the result back
onto the stack.

#### Classroom for GitHub

We're using Classroom for GitHub, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:
* https://classroom.github.com/assignment-invitations/4247c727a1c77eb1982e03bc4083a574

## Instructions

Write your calculator implementation in `rpn.rs`, and your parser
implementation in `parser.rs`. `main.rs` is provided. Hooray! You're finally
writing an executable program!

Like in HW3, your GitHub repository will be pre-populated with a skeleton Cargo
project. You should edit `Cargo.toml` to add yourself as the author.

##### Stack Element Types

Your stack will support two types: `i32` and `bool`. We're starting you off with
a convenient enum to store these, `Elt`, in `rpn.rs`.

##### Operators

You're only required to implement the following operations because ~~they're the
ones that won out in a fight~~ implementing all operations would be tedious:

_Addition_ adds the top two elements of the stack, or type-errors on booleans.

_Negation_ computes logical-not of booleans, and inverts the sign of integers.

_Equality_ compares the top two elements and pushes a boolean onto the stack.

_Swap_ reverses the order of the top two elements on the list.

_Random_ pops a value off of the stack; if that value is an integer `x`, it will
use the `rand` library to generate a random number between `0` and `x` and push
it onto the stack. Otherwise, it type-errors.

_Quit_ exits the program.

We're providing an `Op` type to represent the various operators you may
encounter in the wild, located in `rpn.rs`.

##### Result Aliases

When your calculator hits an error, it should exit. Conveniently, there's also a
handy-dandy provided `Error` enum which defines a few different types of
possible errors: undeflow, type mismatch, syntax, IO, and quit (which is not an
error, but represents an exit value).

Because you're a good software engineer, you're going to create your own
`Result` alias for use in your calculator. Use the provided `Error` enum in
`rpn.rs` to define an alias over `std::result::Result` à la `std::io::Result`.

##### External Crates: `rand`

`rand` is a Rust library which is part of the `rust-nursery` project. (It had
previously been part of `std`, but was moved when Rust decided to downsize its
standard library.) Good thing Cargo makes it so easy to add dependencies!

To use `rand`, you'll need to add it as a dependency to your `Cargo.toml` file.
[The guide][crates-guide] on crates.io (which hosts crates) explains how Cargo
adds and manages project dependencies.
You can find name and version information on [the page for `rand` on
crates.io](https://crates.io/crates/rand)

[crates-guide]: http://doc.crates.io/guide.html#adding-dependencies

You also need to declare it (`extern crate rand`) in your crate's
top-level file (`main.rs`) in order to import and use modules from `rand` in
your project.

##### Stack API

You should define a `Stack` struct, which should define at least the following
public methods:

```rust
/// Creates a new Stack
pub fn new() -> Stack {
    unimplemented!()
}

/// Pushes a value onto the stack.
pub fn push(&mut self, val: Elt) -> Result<()> {
    unimplemented!()
}

/// Tries to pop a value off of the stack.
pub fn pop(&mut self) -> Result<Elt> {
    unimplemented!()
}

/// Tries to evaluate an operator using values on the stack.
pub fn eval(&mut self, op: Op) -> Result<()> {
    unimplemented!()
}
```

You may implement the internals of your stack however you like, but we'd suggest
using a `Vec`.

##### Calculator API

The tokens your parser should recognize and their corresponding interpretations
are as follows:

| Input token | Action                 |
| ----------- | ---------------------- |
| any integer | push Elt::Int(integer) |
| "true"      | push Elt::Bool(true)   |
| "false"     | push Elt::Bool(false)  |
| "+"         | eval Op::Add           |
| "~"         | eval Op::Neg           |
| "<->"       | eval Op::Swap          |
| "="         | eval Op::Eq            |
| "#"         | eval Op::Rand          |
| "quit"      | eval Op::Quit          |

Any other input is considered an error. Your calculator can read multiple tokens
on a single line, and will evaluate them in order. You can parse strings into
integers by using `i32::from_str()`.

We started two functions in `parser.rs` for reading and manipulating input:

`read_eval_print_loop` will do just what it says on the tin: reads from `stdin`,
evaluates some tokens, and then prints out a result. This tin comes with some
~~cookies~~ 🍰 stub code, which prints a prompt and then (`try!`s to) flush
`stdout` so you can read input.

`evaluate_line` is a helper function to `read_eval_print_loop`. It takes the
input `String` and your calculator's `stack` and evaluates the tokens in that
line. Currently, it takes the input string, trims off whitespace from the ends,
and creates a `SplitWhitespace` iterator over the string.

## Tests

Tests have been provided in the starter code. You can add as many tests as you
want (but you don't need to write any). Obviously, you should test your
calculator by using it.

## Submission

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. Make sure it is visible on Github! This is your
submission. (Work must be in the master branch at the due time.)

`cargo test` should work to run all of the tests in your homework on stable
Rust. Make sure you have written tests which cover every one of your functions.

If you have any comments or feedback on this assignment, include them in the
README of your submission.
