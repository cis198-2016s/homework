# Homework 3: "Iterating" On Your Binary Search Tree

**Due 2016-02-10, 11:59pm.**

## Overview

This assignment is modeled after Alexis Beingessner (Gankro)'s [_Learning Rust
With Entirely Too Many Linked Lists_][TMLL] (TMLL).

[TMLL]: http://cglab.ca/~abeinges/blah/too-many-lists/book/

This time, you should look at Chapter 3. We won't do everything in there, but
the content is similar.

#### Classroom for GitHub

We're using Classroom for GitHub, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:
* https://classroom.github.com/assignment-invitations/678e4f3868daaec525ce8f75f23fc53c 

## Instructions

Write your implementation and tests in `src/second.rs`. `lib.rs` is provided.

As before, your code doesn't have to follow an exact interface, but the
function signatures provided will probably work best.

You'll likely want to start off with your solution for HW2 - a lot will carry
over. You'll modify the data structure, `insert`, `search`, and the tests, and
write the code necessary to turn your BST into various iterator types:
`IntoIter`, `Iter`, and `IterMut`.

Like in HW2, your GitHub repository will be pre-populated with a skeleton Cargo
project. You should edit `Cargo.toml` to add yourself as the author.

### Implementation

This _roughly_ corresponds to
[TMLL 3](http://cglab.ca/~abeinges/blah/too-many-lists/book/second.html).
For more details, refer there.

We will use:

* `Option` and `take` (but *not* `map`).
* Type aliases.
* Generic type parameters (`BST<T>`) with trait bounds (`Ord`).
* Named lifetimes and lifetime bounds on type parameters.
* Traits, trait implementations, and associated types.
* Iterators.

We'll essentially be modifying HW2, so you should copy `first.rs` over as
`second.rs` in HW3.

**NOTE:**

[TMLL 3.1][] introduces `map` and anonymous functions and closures. Since we
haven't covered this in class, you can  use `match`es instead of closures
everywhere in this assignment. However, you're welcome to use `map`/closures if
you want the extra practice - simple closures aren't hard.

> Second, `match option { None => None, Some(x) => Some(y) }` is such an
> incredibly common idiom that it was called `map`. `map` takes a function to
> execute on `x` in the `Some(x)` to produce the `y` in `Some(y)`. We could
> write a proper `fn` and pass it to `map`, but we'd much rather write what to
> do *inline*.

#### Details

As in TMLL 3:

* Convert `BST` and `Link` to take a generic element `T` instead of `i32`.

* Replace the `Link` type with a type alias for `Option<Box<Node<T>>>`, because
  your life has felt strangely lacking in angle brackets recently. Then, update
  all of the code which uses `Link`. You may not have used `mem::replace` in
  HW2, in which case you won't need `take` yet.

  * If you want, take a look through the [`Option` documentation][optdoc] to
    find useful methods.

[optdoc]: https://doc.rust-lang.org/std/option/enum.Option.html

* Now that `Link` is a type alias and not a struct, you cannot directly `impl`
  methods for it, because you don't own the type `Option`. However, you do not
  feel constrained by the ~~silly~~ very serious and important rules that Rust
  imposes on your life, so you can work around this by defining a generic trait
  `InsertSearch<T>` which provides your two functions `insert(&mut self, e: T)
  -> bool` and `search(&self, e: T) -> bool`.

  * Proceed to exercise your unbounded power by implementing `InsertSearch`
    for `Link<T>`, by adapting the functions you wrote for HW02 to be generic
    over `T`. Make sure your old tests still pass.

* Now you will transform your ordinary BST into an extraordinary BST iterator!
  We're going to use the [`Iterator`] trait.

  * Build yourself an `IntoIter` struct like the one in [TMLL 3.4][].

    * Since this is a BST, and not a list, we're going to cheat a bit and
      just iterate over the rightmost edge of the tree, since that's less
      annoying. (If you feel like it, you can try doing an in-order
      traversal of the tree instead. We can't guarantee that you can do it
      with the material we have covered so far, so try the cheaty version
      first!)

    * Implement the [`Iterator`][] trait for `IntoIter`. This requires an
      associated type, `type Item`, and an implementation of
      `next(&mut self) -> Option<Self::Item>`.

  * Instead of implementing `BST::into_iter` as a plain member function,
    we're going to do something _way_ cooler:

    * [`IntoIterator`][] is a trait with one method, `into_iter`. This is the
      sugar that fuels Rust's `for` loops. Go ahead and `impl IntoIterator for
      BST`. Don't forget: this trait requires you to declare associated types!
      Read the [docs][`IntoIterator`] for the deets.

[`Iterator`]: https://doc.rust-lang.org/std/iter/trait.Iterator.html
[`IntoIterator`]: https://doc.rust-lang.org/std/iter/trait.IntoIterator.html

  * BAM! Now your BST can harness the power of `for` loops. Try this:

```rust
let bst = BST::new();
bst.insert(1);
bst.insert(2);
bst.insert(3);

for elt in bst { // calls bst.into_iter()
    println!("{}", elt);
}
```

  * But your power is yet incomplete. What about borrowed iteration? Implement
    an `Iter` struct, as described in [TMLL 3.5] (similar to `IntoIter`).

    * Again, instead of implementing `BST::iter`, we're going to lord our
      superiority over Gankro's tutorial and use `IntoIterator`. But you can't
      just implement a trait _twice_ for the same struct; that would be absurd.
      And confusing. It would break so many rules. Instead, we'll implement this
      for `&BST`.

    * You're going to need named lifetimes here! To start, you have to
      implement `impl<'a, T> IntoIterator for &'a BST<T>`.

    * This is what allows:

```rust
for elt in &bst { // calls bst.into_iter()
    println!("{}", elt);
}
```

  * Finally, we'll move on to [TMLL 3.6][], `IterMut`. You know what you need
    to do.

```rust
for elt in &mut bst { // calls bst.into_iter()
    println!("{}", elt);
}
```

[TMLL 3.1]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-option.html
[TMLL 3.4]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-into-iter.html
[TMLL 3.5]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-iter.html
[TMLL 3.6]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-iter-mut.html

### Tests

Test all three of your iterators. To do this, you should check two things:

* For a BST `bst`, you should be able to compile a for loop over
  `bst`, `&bst`, or `&mut bst`.

* The values returned by the iterator should be correct. You can explictly
  get an iterator (with, for example, `(&mut bst).into_iter()`), then
  `assert_eq!` the values returned by `next()`.

### Submission

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. Make sure it is visible on Github! This is your
submission. (Work must be in the master branch at the due time.)

`cargo test` should work to run all of the tests in your homework on stable
Rust. Make sure you have written tests which cover every one of your functions.

If you have any comments or feedback on this assignment, include them in the
README of your submission.
