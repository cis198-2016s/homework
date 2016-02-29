# Homework 06: A Bulletin Board System (BBS)

**Due Sunday, 2016-03-06, 11:59pm.** (TODO)

The internet is the beginning of the hyper-connected future. Isn't it great?
We're working to create a new _wireless_ social network, where people can
communicate with their friends over the internet! We think the next big product
in this space will be bulletin board systems, where people in all different
places can gather and share information throughout the world.

You've recently been hired as an engineer. Welcome to the team! Since we've
heard that you have a lot of experience using Rust, you've been given
responsibility for this BBS project that we've just started. Good luck!

#### Classroom for GitHub

We're using Classroom for GitHub, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:

https://classroom.github.com/assignment-invitations/c674a59f072e68a140e54599fcf35e8f

### [`hyper.rs`][]

[`hyper.rs`]: http://hyper.rs

> Hyper is a fast, modern HTTP implementation written in and for Rust. It is a
low-level typesafe abstraction over raw HTTP, providing an elegant layer over
"stringly-typed" HTTP.

Hyper is the beginning of the future. Hyper contains both a server, which
manually handles requests that it receives, and a client, which can build HTTP
requests and parse HTTP responses. You will use both in this assignment.

## lib

We're going to build this BBS in a library crate, so it can be easily deployed
to others who want to use it. This crate will contain `lib.rs` as before, which
defines common functions and types used in the rest of your project.
Additionally, the crate contains several binaries (located in `src/bin`). Each binary
is a standalone Rust file, which uses the library as if it were an external
crate.

Here's what's defined in `lib.rs`:

- `Message`: a struct representing a post on the bulletin board, containing the
  username and their message.
- `UserClient`: a struct storing a username, the server address they make posts
  to, and a `hyper::Client`, which can be used to make multiple requests to a
  server.
  - `get_content`: a method which builds a GET request to the stored server
  address.
- Some constants which are shared between binaries.
  - `SERVER_ADDR`, `BOT_ADDR`: the addresses that the server and bot will listen
    on.
  - `HTML_ADDR`: the addresses the BBS is visible at (i.e. the
    address you'd visit with browser)
  - `HTML_HEADER`, `HTML_DATA`, `HTML_FOOTER`: the local files which the server
    uses to generate the page.

## `bin/server`

Provided: A basic web server which can respond to GET requests at
http://127.0.0.1:1980 and serves a static web page. It generates this static web
page by concatenating two files (the header, and footer).

* You may need to change the port numbers from 1980 to others if you are working
  on Eniac, to avoid colliding with other servers.

This is the _future_, though. Static webpages are so _nineties_. Add to the
existing request handler (`req_handler`) the ability to receive posts through
POST requests. The body of a POST request will contain a JSON-encoded `Message`
(defined in `lib.rs`). You should append each post to the bulletin board's data
page (`HTML_DATA`) when it's received. You should also update the GET request
handler to include the posted messages when it generates a page when it
generates a page.

* For more context on RESTful APIs and the difference between GET and POST
  requests, you can read this [StackOverflow page][restful].

[restful]: https://stackoverflow.com/questions/671118/what-exactly-is-restful-programming

Notice that, since the `bbs` library is external to this server, you need to
declare it with `extern crate bbs` at the top of your file, and import parts as
needed.

## `bin/client`

Provided: A client which initializes a user and makes a GET request to read the
current bulletin board.

_Boring._ The user of this software should be able to post a message to the
bulletin board. Otherwise, this wouldn't be an innovation at all!

We'd like to provide our users with a clean, modern command-line interface for
making posts to our bulletin board. It's up to you to design this. For example,
you may retain the current command line interface (where the user specifies
their username), and allow the user to post messages to the BBS by reading from
standard input.

## `bin/bot`

Oh no! You've put all of this hard work in, and your bulletin board still
doesn't have any users. ~~Because you have no friends,~~ To help build activity
and user interest, you decide to build a bot service which will automatically
respond to certain types of messages on the board. This is exactly the sort of
fun, exciting feature that people are looking for on the Web.

Add a new binary to your BBS crate which contains a bot.

This bot listens for TCP connections on port 1981 (as defined in `lib.rs`).
Whenever the server receives a new post, it will make new connection to this bot,
relaying the contents of the post. The bot will read the post and determine if
it should respond.

This is the future, so of course your bot will have to be connected to the
internet. When the bot receives a message of the form "choose x y z", it will
make a query to random.org's [HTTP API][random-api], asking for a value between
1 and the length of the options. You can use `hyper::client` to receive and
parse these queries. Then, the bot should post a message back to the server with
the appropriate choice (e.g. for a value of 2, your bot would post the message
"y"). Bam. Random.org. _The future_.

* You can easily build these query URLs with a format string. A simple example
  (as given [here][random-api]) will generate ten integers in base 10, between 1
  and 6, returned as a plaintext page:
  https://www.random.org/integers/?num=10&min=1&max=6&col=1&base=10&format=plain&rnd=new

You are the head engineer on this project, so you are free to build any sort of
bot functionality you want. If you want to do something different, just explain
what you've done in your README.

[random-api]: https://www.random.org/clients/http/

## Feature List

To recap, here are all the features you should implement:

- In `server.rs`:
  - Handle `POST` requests in `req_handler`
  - Update the `GET` handler in `req_handler` to generate a page containing the
      post data
- In `client.rs`:
  - Create a command-line interface to allow users to easily send data to the
      server
- In `bot.rs`:
  - Listen on port 1981 for incoming TCP connections to determine if new posts
      have been made to the BBS
  - Create a function to randomly choose "x", "y", or "z" from posts of the form
      "choose x y z" using random.org's HTTP API

## Submission

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. Make sure it is visible on Github! This is your
submission. (Work must be in the master branch at the deadline.)

If you have any comments or feedback on this assignment, include them in the
README of your submission.
