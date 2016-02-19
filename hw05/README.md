# Homework 05: The ~~Fun-Time Reference Sharing~~ _Darkest_ Dungeon
### It's not just a phase, _Mom_!

**Due Friday, 2016-02-26, 11:59pm.**

For questions, please post on Piazza (Penn students) or Google Groups (other).
Links on course homepage.

## Overview

You have found yourself standing at the entrace of a large, mysterious,
~~smelly~~ castle. You must have taken a wrong turn when you were wandering
through that dark, imposing, werewolfy forest. Why were you doing that?
Nevermind, it's not relevant. Just go on inside. There's treasure. I think.
There's definitely at least gold. This wouldn't be an adventure game if you
couldn't even find gold!

Ahem. Let me try this again:

> _Wander forth for promises of grand treasures hidden in glamorous bejeweled
chests and decrepit iron maidens, dangerous traps filled with poisonous spikes.
Climb over the forgotten corpses of the fallen adventurers who have come before
you yet were not as blessed with luck._

Wait, seriously, _where_ is that smell coming from?

#### Classroom for GitHub

We're using Classroom for GitHub, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:

* https://classroom.github.com/assignment-invitations/d7fc86f713d77fa96deed930a9e75244

## Instructions

- In this assignment we are going to build a text adventure game!

Okay, we lied. The castle doesn't exist yet. All you have is some poorly
written down map that someone shoved into your hand earlier. They didn't even
draw it out properly! It's written in some weird... "JSON" format.

- Finish the JSON parser. The provided code parses a JSON file to create a
  series of rooms connected by hallways. See below for more detail.

- Each room contains a set of Curios. The generation of Curios for a given room
  is provided.

[*Look around you*][]. What can you see? Nothing! You haven't implemented eyes
yet. That doesn't make any sense. What are you doing. Nevermind, nevermind. Stop
asking questions.

[*Look around you*]: https://www.youtube.com/watch?v=gaI6kBVyu00

- Print out a description of a room when you enter it, containing  adjacent
  rooms accessable from your current location.

We promised you gold, and so there shall be gold! Some of these rooms contain
treasure and glory! Chests contain gold; food provides nourishment and restores
health. Be sure to look closely: some curios, such as iron maidens and fallen
adventurers, contain yet more items within them.

But beware! There is danger afoot. When you encounter a spike trap or iron
maiden, it will deal damage to your person.

- Use all items in a room when you enter it, and output to the player
  ~~a tale of their misery~~ a description of what happened to them. All items are
  handled immediately but death visits at the end of the turn, so it is possible
  to take fatal damage from a spike trap but immediately cure yourself with a
  fistful of food to avoid death. Gold is added to your inventory. Items can
  only be used once, so they should be removed from a room once used.

It wouldn't be an adventure if you were stuck in the entrance. You'd probably
die of boredom before you even died of thirst.

- Implement `go [destination]` to move from your current location to any
  room connected by a hallway.

Now that you have the freedom of movement, you've finally come to a great,
terrible, disgusting realization. You've remembered the legend of *the Wumpus*.

The Wumpus is a large, repulsive, oozing monster from which that putrid smell
emanates. The Wumpus will consume your being if you happen upon the room where
it makes its lair. However, you are no defenseless peasant; you have prepared
with your mighty bow and arrow. If you shoot an arrow toward the room a Wumpus
lies in, it will surely kill the Wumpus, because you are a mighty warrior and
all of your arrows fly true. Or maybe because the Wumpus is so big you can't
possibly miss.

- Implement `shoot [destination]` to shoot an arrow into a room and kill the
  Wumpus if it's there.

When you've killed the Wumpus, congratulations! You have freed this castle from
its terrible and malodorous curse. All the land rejoices in your honor. You are
crowned as the exalted ruler for the people whose lives you have saved. There is
no end of riches and luxury for you. Or something. Maybe you just go home and
smell the roses.

## Demo

If this doesn't make any sense, or you aren't familiar with text adventures, you
can play this game on Eniac:

```rust
~cis198/local/bin/hw05 ~cis198/local/share/hw05/castle.json
```

### How To Play

After building your project, you can run it with `cargo run
[castle_description.json]`. A sample castle is provided in `data/castle.json`.

The available commands are `go [room]`, `shoot [room]`, and `quit`. Quit prints
your score, whether or not you've won, and exits the game. Dying, when your
health falls below 0, also results in exiting the game.

Players have a location (`Rc<RefCell<Room>>`, health (`i32`), gold (`i32`), and
an account of whether they won (`bool`). Players can `Go` or `Shoot` a room.

Movement between rooms should be implemented by replacing the stored
`Rc<RefCell<Room>>` contained by the player.

### Castle Structure

A room in this castle contains a name (`String`), a list of contents
(`Vec<Curio>`), a list of halls (`Vec<Rc<Hall>>`), and the possibility of a
Wumpus (`bool`). Halls each connect two rooms (two
`Option<Rc<RefCell<Room>>>`s).

See the provided JSON file (`data/castle.json)`: Rooms are defined by name, the
number of randomly-generated curios, and whether the wumpus is in that room.
Halls are defined as tuples (in as much as JSON allows tuples) between room
numbers. Rooms are numbered in the order they are defined. Room names should be
unique.

The JSON parsing is provided; you will start by finishing room and hallway
generation.

## Testing

No tests! Yay!

We're not providing any tests nor expecting that you write any. We'll grade your
game by simply playing it.

## Questions

Edit the README in your project with a (brief!) explanation of how `Rc` and
`RefCell` are used and why they are necessary in this game.

## Make your game more game

Apply as much creativity as you want to your game. Add your own maps,
flavortext, additional curios, miscellaneous player effects. Extremely arbitrary
numbers of brownie points (which can be redeemed for real brownies!) will be
distributed for making your game more fun.

Please do not edit the structure of Rooms and Halls. Otherwise, you're missing
the point of this assignment!

Highlight any interesting additions that you've made in your README.

## Submission

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. Make sure it is visible on Github! This is your
submission. (Work must be in the master branch at the due time.)

`cargo test` should work to run all of the tests in your homework on stable
Rust. Make sure you have written tests which cover every one of your functions.

If you have any comments or feedback on this assignment, include them in the
README of your submission.
