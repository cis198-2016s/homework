# Homework 01: Rust Finger Exercises

**Due 2016-01-27, 11:59pm.**

## Overview ##

In this assignment, you'll get your first crack at writing some short functions
in a Rust library, building and testing using Cargo, and writing modules. This
assignment isn't intended to be especially difficult, but will make sure you're
set up properly in the Rust ecosystem, and will give you some good experience
using the Rust compiler.

## Provided Code ##

Just one file, `tests_provided.rs`, which contains several test cases. We're asking you
to write everything else from scratch, but we'll give you function signatures below,
as a starting point.

## Part 01: Cargo ##

Don't clone your GitHub repository just yet.
To start off, let's create a new Rust library: `cargo new hw01`.

If you are not already in a git repository when you create your project, Cargo
will create a git repository (and `.gitignore`) for you. Then, you can add this
your GitHub repository as a git remote, with something like:

```
git remote add origin git@github.com:cis198-2016s/hw01-<username>.git
git push -u origin master
```

(If you're not using SSH for GitHub, you need to use the HTTPS URL of your repository.)

Cargo creates this directory structure for you:

```
hw01
├── .git/
├── .gitignore
├── Cargo.toml
└── src
    └── lib.rs
```

You can build this project from anywhere in the project tree with `cargo build`.
Remember to compile periodically as you work.

### Modules ###

Before you go any further, a word on modules and crates.

Crates are any Rust library or package. Modules are the inner logical sections
of a crate.

You will want to organize each section of your crate into a different module,
so consumers can only import the parts that they need. You could do this in
`lib.rs`:

```rust
pub mod problem1 {
}

pub mod problem2 {
}

pub mod problem3 {
}

pub mod problem4 {
}

// ...
```

But this gets unwieldy pretty fast. You can instead put these modules in separate
files:

```
hw01
├── Cargo.toml
└── src
    ├── problem1.rs
    ├── problem2.rs
    ├── problem3.rs
    ├── problem4.rs
    └── lib.rs
```

Every `.rs` file defines a module that is the same as its filename, so
`problem1.rs` implicitly defines the module `problem1`.

Crates are organized into trees of files, where one file is the root, typically
`src/lib.rs` (or `src/main.rs`). To include a module, you need to declare it in
the crate root. To add `problem1` as a module, add the line `pub mod problem1;`
to the top of `src/lib.rs`. (This is equivalent to defining a module in the file
itself, with `pub mod problem1 { ... }`.) Until you add this directive, Cargo will
not try to build `problem1` part of your crate. The `pub` keyword in this
directive exposes the module `problem1` to any other crate that imports your
library. You can omit `pub` to leave modules private; however, if a function is
not exported or used internally, it will emit a dead code warning.

Within a module, all members (functions, types, submodules) are private by
default. The `pub` keyword can also be used to make any of these available from
outside of the module.

```rust
// problem1.rs

/// Functions are private (only available to this module) by default.
/// Use the `pub` keyword to mark this function as public.
pub fn sum(slice: &[i32]) -> i32 {
    // ...
}
```

There are a few different ways to import items from a different module:

1. To add `sum` to the scope of a file, add the line `use problem1::sum` to the
   top of the file. You can call the function with `sum()`. Try to import only
   what you need to use.

2. To import multiple things from a module, use curly braces:
   `use problem1::{sum, dedup};`

3. To import all items from a module, write `use problem1::*`.

4. To import an entire module, write `use problem1;`. This form requires you to
   qualify members of the module with the module name (e.g. `problem1::sum()`).
   This is more verbose but does not pollute the namespace of your scope.

We provided a `tests_provided.rs` file with a few test cases to start you off with. You
should add this to your library as a separate module, as you did with each
`problemX` module (but the tests don't need to be `pub`).

You can read more about modules in the Rust book
[here](https://doc.rust-lang.org/book/crates-and-modules.html).

## Part 02: Basic Functions ##

### Preface ###

For consistency, we ask that you put each problem into its own file, and name
the file as `problem1.rs`, etc. Remember to declare each module at the top of
`lib.rs` as well!

### Problem 01: Vector & Slice Manipulation ###

*Vectors, iteration, pass-by-ref, mutability, function pointers.*

Create a new module in your library named `problem1` inside `problem1.rs`.

To get a first taste of basic Rust, complete the following three functions.
Note that all of these functions take their arguments by reference, rather than
by value.

Don't use any of the standard library methods on the `Vec` class which implement
the target behavior, since using them would defeat the point of this exercise
:). (However, basic functions such as `contains()` and `push()` are fine).

```rust
/// Computes the sum of all elements in the input i32 slice named `slice`
pub fn sum(slice: &[i32]) -> i32 {
    // TODO
    unimplemented!();
}

/// Deduplicates items in the input vector `vs`. Produces a vector containing
/// the first instance of each distinct element of `vs`, preserving the
/// original order.
pub fn dedup(vs: &Vec<i32>) -> Vec<i32> {
    // TODO
    unimplemented!();
}

/// Filters a vector `vs` using a predicate `pred` (a function from `i32` to
/// `bool`). Returns a new vector containing only elements that satisfy `pred`.
pub fn filter(vs: &Vec<i32>, pred: &Fn(i32) -> bool) -> Vec<i32> {
    // TODO
    unimplemented!();
}
```

#### Testing

Before you move on, take a look at `tests_provided.rs`.

At the top of this file, there's a `#![cfg(test)]` attribute. `cfg`, short for
"configuration", is part of how Rust handles conditional compilation. This is
similar to (but way better than) the use of `#ifdef`s and include guards in C.
In this case, `#![cfg(test)]` tells the compiler that this module is not to be
compiled unless the `--test` flag is used with `rustc` (`cargo test` adds this
flag under the hood). This is handy because it means that you don't have to
waste time recompiling your tests every time you build unless you're actually
going to run them.

All of the functions in `test.rs` are annotated with the `#[test]` attribute;
this tells the compiler that they're tests. Test functions must have the
signature `fn() -> ()`, or else they will not compile. Tests will be run when you
invoke `cargo test`.

Any test which doesn't cause a `panic!` is considered to pass. You should use
[`assert!`][assert] or [`assert_eq!`][assert_eq] to check guarantees and
equality (respectively).

[assert]: https://doc.rust-lang.org/std/macro.assert!.html
[assert_eq]: https://doc.rust-lang.org/std/macro.assert_eq!.html

As an aside, the `cfg` attribute has many other uses, like knowing
which OS or architecture you're compiling for.

We have provided a few tests to start you off with. You should add at least one
non-trivial test for each function that you implement. Write these in a new
module, `tests_student.rs`, and declare the module in `lib.rs`.

### Problem 02: Matrix Multiplication ###

*Vectors, iteration, pass-by-reference, structs.*

Create a new module in your library named `problem2` inside `problem2.rs`.

We define a `Matrix` as a type alias to `Vec<Vec<f32>>`. Write a function that
takes in two `Matrix`es by reference and returns the product `mat1 * mat2`. The
function signature is provided below.

You should make sure that the two input matrices are actually compatible.
Remember that you can't multiply two matrices if the number of columns in the
first matrix is not equal to the number of rows in the second matrix. Use
`assert!` or `assert_eq!` to `panic!` if this condition is not met.

(Of course, crashing is bad - we'll learn about fixing this later.)

```rust
/// Represents a matrix in row-major order
pub type Matrix = Vec<Vec<f32>>;

/// Computes the product of the inputs `mat1` and `mat2`.
pub fn mat_mult(mat1: &Matrix, mat2: &Matrix) -> Matrix {
    // TODO
    unimplemented!();
}
```

### Problem 03: Sieve of Eratosthenes ###

*Vectors, iteration, mutability...*

Create a new module in your library named `problem3` inside `problem3.rs`.

The [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
is used to find all primes below some given number (`n`), and is an efficient
way to find small primes.

Iterate through the numbers from `2` to `n`. For each number `i`:

1. If `i` has been crossed-out from previous iterations, skip it.
2. If `i` isn't crossed-out yet, then it is prime.
   Cross-out all multiples of `i` from `i*i` to `n`. These are non-prime.

Head over to the Wikipedia page for a more detailed description and some zesty
examples.

Your function will take a number, `n`, and return a list of all prime numbers
less than `n`. (Notice that you can't use a Rust array in this case, since you
don't know the length at compile time.)

```rust
/// Find all prime numbers less than `n`.
/// For example, `sieve(7)` should return `[2, 3, 5]`
pub fn sieve(n: u32) -> Vec<u32> {
    // TODO
    unimplemented!();
}
```

### Problem 04: Towers of Hanoi ###

*Mutability, type aliases, vecs, tuples, enums.*

[The Towers of Hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi) is a
classical mathematics and computer science puzzle. Imagine you have three pegs,
one of which holds a stack of discs in increasing size from bottom to top. Your
goal is to move all of the discs from the first peg to the third peg, using the
second peg as an intermediate. Your only restrictions are that you may only move
one disc at a time, and a disc may only be placed on a disc larger than it (or
on an empty peg). Check out the Wikipedia page for more details and snazzy
animations.

In this instance, we've provided you with an enum type containing the possible
names of `Peg`s and a type alias defining a move between two pegs.

This function will take in a number of discs, and the names of the three
pegs, and return a vector of `Move`s.

```rust
/// #[derive(...)] statements define certain properties on the enum for you for
/// free (printing, equality testing, the ability to copy values). More on this
/// when we cover Enums in detail.

/// You can use any of the variants of the `Peg` enum by writing `Peg::B`, etc.
#[derive(Clone, Copy, Debug, Eq, PartialEq)]
pub enum Peg {
    A,
    B,
    C,
}

/// A move between two pegs: (source, destination).
pub type Move = (Peg, Peg);

/// Solves for the sequence of moves required to move all discs from `src` to
/// `dst`.
pub fn hanoi(num_discs: u32, src: Peg, aux: Peg, dst: Peg) -> Vec<Move> {
    // TODO
    unimplemented!();
}
```

## Submission

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. **Make sure it is visible on Github!** This is your
submission. (Work must be in the master branch at the due time.)

Your repository should look like this:

```
[git root]
├── Cargo.toml
└── src
    ├── lib.rs
    ├── problem1.rs
    ├── problem2.rs
    ├── problem3.rs
    ├── problem4.rs
    ├── tests_provided.rs [same as given]
    └── tests_student.rs [your tests]
```

`cargo test` should run all of the tests in your homework. Make sure you have
written at least one test for each function you have written.
