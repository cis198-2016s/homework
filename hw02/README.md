## Homework 2: A Mediocre Binary Search Tree ##

This assignment is modeled after Alexis Beingessner (Gankro)'s [_Learning Rust
With Entirely Too Many Linked Lists_][TMLL] (herein referred to
as TMLL, because that's much easier to pronounce, right?)

[TMLL]: http://cglab.ca/~abeinges/blah/too-many-lists/book/

Even though we're implementing a [BST][], you should read Chapters 1-2
(introduction and first linked list); they're a great place to start.
Gankro's writing is really fun to read and the content is enlightening.

[BST]: https://en.wikipedia.org/wiki/Binary_search_tree

### Instructions

Write your implementation in the `src/first.rs` file. You'll also write tests
in this file. The main file, `lib.rs`, is rather boring and done for you.

Your code doesn't have to follow any exact interface, just the guidelines
below. You'll write the data structure, `insert`, `search`, and tests.

Unlike in HW1, if you create your repository using Classroom for GitHub (link
on Piazza), your Git repository will be pre-populated (you won't need to use
`cargo new`). Just clone it to get going. (See the `starter` subrepo for the
starter repository contents.)

#### Aside: Clippy

[Clippy][] is a compiler plugin which runs [lints][] on your code. It only
works in nightly Rust, however, so it's not enabled by default here. If you
feel like trying it out, switch to the nightly channel
(e.g. `multirust override nightly` while inside the project directory),
then build with the `clippy` feature:

```
cargo build --features clippy
cargo test --features clippy
```

This is recommended as it will help catch some stylistic errors as you learn
the language. (But, for ease of grading, please be sure to submit code which
works on Rust stable.)

[Clippy]: https://github.com/Manishearth/rust-clippy
[lints]: https://en.wikipedia.org/wiki/Lint_%28software%29

(If for some reason Clippy causes problems for your stable Rust build, just
remove the `[dependencies]` and `[features]` sections from `Cargo.toml` and
the Clippy-related lines at the top of `lib.rs`.)

### Implementation

This _roughly_ corresponds to
[TMLL 2](http://cglab.ca/~abeinges/blah/too-many-lists/book/first.html).
For more details, refer there.

As in TMLL 2:

* Write a `BST` type similar to the one in TMLL 2: a `pub struct` with a `root`
  element of type `Link`. Implement `BST::new()` which creates an empty BST.
    * This will be the only `pub struct`. The others are implementation details.
* Define `Link` as an `enum` with two instances: `Empty` and `More`, where
  `More` contains a boxed `Node`.
* Define `Node` as a `struct` containing an `i32` element. Instead of a single
  `next` element, it should also contain two child `Link`s: `left` and `right`.
* Add `#[derive(Debug)]` before each of the three types. This allows you to
  debug-print a value, e.g.: `println!("{:?}", bst);`
    * To be able to see printed output of successful tests, use
      `cargo test -- --nocapture`.

Now, instead of TMLL's `push` and `pop`, we'll implement:

* (`pub`) `bst.insert(i32) -> bool`: Insert an element into the BST. Return true if
  successful, or false if the element was already in the BST.
* (`pub`) `bst.search(i32) -> bool`: Search for an element in the BST. Return true iff
  the element was found.

#### Hints from our reference implementation

You should need: `struct`, `enum`, `impl`, `match`/`ref`/`ref mut`, `Box`,
`&`/`&mut`, dereferencing (`*`).

You should not need: named lifetimes, `Option`, `Vec`, `use`.

In our reference implementation, `BST::insert` and `BST::search` are short
functions which just call longer, recursive member functions of `Link`:

* `link.insert(i32) -> bool` (~25 lines)
  * If inserting into an empty link, place the element in this link (making it
    no longer empty).
  * If inserting into a non-empty link:
    * return `false` if the element is in this node; otherwise,
    * recurse to the left if the new value is less than the node's value
    * recurse to the right if the new value is greater than the node's value
  * You may not end up needing `mem::replace` like in TMLL.

* `link.search(i32) -> bool` (~15 lines)
  * If searching an empty link, return `false`; the element can't be found.
  * If searching a non-empty link:
    * return `true` if the element is in this node; otherwise,
    * recurse to the left if the target value is less than the node's value
    * recurse to the right if the target value is greater than the node's value

### Tests

You can run tests with `cargo test`. To show any printed output of successful tests, use
`cargo test -- --nocapture`.

Tests should be defined in the way specified in
[TMLL 2.6](http://cglab.ca/~abeinges/blah/too-many-lists/book/first-test.html).
This keeps unit tests close to the code that they test.

```rust
#[cfg(test)]
mod test {
    use super::List;

    #[test]
    fn test_push_pop() {
        // ...
    }
}
```

You should test `search` and `insert` in various orders, for both true and
false results. Use `assert!()` and/or `assert_eq!()`. They don't have
to be any more complex than the tests in TMLL 2.6.

### Submission

Just like in homework 01, commit and push your work to the master branch of your
Classroom for Github repository for this HW. Make sure it is visible on Github!
This is your submission. (Work must be in the master branch at the due time.)

`cargo test` should work to run all of the tests in your homework on stable
Rust. Make sure you have written tests which cover every one of your functions.
