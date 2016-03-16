# Homework 7: A Multithreaded Chat System

**Due Wednesday, 2016-03-23, 11:59pm.**

Okay, you admit it - a BBS isn't exactly the most modern web technology of all
time. Inspired by the rising success of this newfangled "AOL Instant Messenger"
thing all the kids are using, you've decided to make your own chat service!

In this homework, you'll write your own multithreaded, multi-user, IRC-esque
chat service! And as a bonus, it'll run in the browser using WebSockets!

#### Classroom for GitHub

We're using Classroom for GitHub, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:

* https://classroom.github.com/assignment-invitations/8380d7698ae30e972cf46d5c80daa2a8

## Background

**This assignment should work on [any modern browser](http://caniuse.com/#feat=websockets); tested on Firefox and Chrome.**

### WebSockets

WebSockets are a web technology for allowing JavaScript in webpages to connect 
back to servers using a bidirectional data stream. WebSockets behave very
similarly to TCP streams (but are message-based).

We will be using the `rust-websocket` library. You'll definitely need to take
a look at some of the documentation and examples:

* [GitHub repo](https://github.com/cyderize/rust-websocket)
* [Documentation](http://cyderize.github.io/rust-websocket/doc/websocket/)
* [Example server](https://github.com/cyderize/rust-websocket/blob/master/examples/server.rs)

### Multithreaded Networking

In this assignment, we'll be using threads to easily handle several network
clients at once. One way to do this is to create one thread for each client.
As clients connect, a thread is spawned to manage that particular connection:

```rust
// fn listen
for connection in server {
    // Spawn a client_thread.
}
```

In this paradigm, each _client thread_ will _block_ as it waits for data to
come in from its respective client. This can be expressed very elegantly using
iterators: every time a message comes in, the loop runs once. When the channel
closes, the loop terminates.

```rust
// fn client_thread
for message in client_recv.incoming_messages() {
    // Handle message; relay via MPSC channel.
}
```

In the _relay thread_, each message received via the MPSC channel should be
sent to all of the clients (including the originator). Once again, iterators
allow us to use the MPSC receiver very nicely:

```rust
// fn relay_thread
for action in relay_mpsc_recv {
    // Send message to all clients.
}
```

**Aside:** For many applications, the one-thread-per-client model does not work
well. For systems which will have many (thousands or more) clients, spawning
thousands of threads is very inefficient. In these cases, a more complex system
will typically be used; for example, several threads (usually about one per
core) might run in a thread pool, where each _asynchronously_ handles many
clients. That is, each thread will periodically poll for incoming data from
each client (_non-blocking_), rather than waiting for incoming data from a
single client (_blocking_). For `rust-websocket`, there is a discussion
[here](https://github.com/cyderize/rust-websocket/issues/6).

### IRC Principals

IRC (Internet Relay Chat) operates on very simple principals:

* Maintain a list of all of the currently connected clients.
* For each message that comes in, relay it back out to all of the clients.

We won't be implementing IRC precisely, but we will use the "relay" idea.

## Instructions

`main.rs` and `webpage.rs` are provided for you; they just serve a static HTML
webpage over **port 1980**. To access this, just run the server (`cargo run`)
and open [localhost:1980](http://localhost:1980/) in a web browser.
The static webpage is also written for you.

Your job is to write `chatserver.rs`. In `chatserver::start`, you should spawn
a thread to listen for incoming WebSockets connections on **port 1981**.
We've also given you a (de)serializable `enum ChatAction` - don't modify this;
the JavaScript code depends on it!

The [example server](https://github.com/cyderize/rust-websocket/blob/master/examples/server.rs)
will be an important resource - you'll use a lot of the same boilerplate code.
(Note: we aren't using a protocol; the protocol response isn't necessary.)

Now, go check out the comments left in `chatserver.rs`!

## Submission

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. Make sure it is visible on Github! This is your
submission. (Work must be in the master branch at the deadline.)

If you have any comments or feedback on this assignment, include them in the
README of your submission or post on Piazza or the Google Group.
