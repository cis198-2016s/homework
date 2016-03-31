# Final Project

This final project gives you the freedom to do whatever you've been dying to
do with this fancy new programming language you've just learned.

## Schedule

* Tue 3/29 - Ideas Due (on Piazza megathread) (for participation credit)
* Thu 3/31 - Proposal Draft Due (PDF via email)
* Fri 4/01 - Proposal Meeting (by appointment)
* Wed 4/06 - Proposal Presentation (in class) (4 min)
    * Please also send: final proposal, repository link, and documentation link.
* Wed 4/13 - Milestone 1 Presentation (in class) (2 min)
* Wed 4/20 - Milestone 2 Presentation (in class) (2 min)
* Wed 4/27 - Final Presentation (in class) (6 min)
* Wed 4/27 - Final Report (PDF via email)

**IMPORTANT:** After your proposal is submitted, we want to talk to you before
the proposal presentation to discuss and finalize. There will be a sign-up
sheet for meetings, probably on Friday, April 1.

## Grading

This project is worth 40% of the final course grade.

* 15%: Proposal and Presentation
* 20%: Milestone 1 Presentation and Status
* 20%: Milestone 2 Presentation and Status
* 45%: Final Presentation, Status, and Report

**Important:** "Status" grading will be based primarily on your documentation.
Write in-depth documentation! You'll need to use rustdoc to generate
documentation before each presentation
(see below: [Documentation](#documentation)).

## Project Guidelines

* Groups may be 1-3 people, 2 preferred. Collaborate via GitHub (obviously).
  Make your own repository if applicable.

* You can either make your own project or contribute to an existing open-source
  project. If you do the latter, make sure that the project owners are okay
  with this. They will expect very high-quality contributions - so don't drop a
  half-done pull request on them.

* If you make a new project, we _prefer_ (but don't require) things that
  haven't been done in Rust before.

* Your idea needs to be doable in 3 weeks. This will be all of your
  198 work during those weeks, so schedule accordingly.

* We expect you to do about (3 weeks * 0.5 credit) worth of work per person -
  but what that means is subjective. More work will mean a more impressive
  project - but it's much more important that you finish!

* If your idea is in danger of being too big, you need to have a plan for how
  to scale it back. Carve out a reasonable subset of the idea as your baseline,
  and have additional goals on top of that.

* We would really like your project to be open-source! You don't have to if you
  really don't want to, but contributing to the Rust ecosystem is ideal.
  You don't need to pick licensing immediately, but consider Apache and MIT.

* If you have no ideas you like, you might consider putting out a call for
  project ideas to the Rust community (e.g. via Reddit).
  (If you take a community idea, your project must be open-source.)

### Ideas

These are our ideas - none of them have been investigated and you don't
have to use any of them.

* http://www.ncameron.org/rust.html
* https://www.rust-lang.org/contribute.html
* https://www.reddit.com/r/rust/comments/3bjl53/rust_language_project_ideas/
* https://github.com/rust-lang/rust/blob/master/CONTRIBUTING.md
* https://github.com/servo/servo/blob/master/CONTRIBUTING.md
* https://github.com/rust-lang-nursery
* https://github.com/redox-os/redox#contributing

* Parallel computation (e.g. scientific computing or ray tracing)
* Other fast scientific computing
* Safe(!) bindings for a C API
* Rewriting slow parts of other programs (e.g. Python/Ruby)
* Some interesting distributed system
* Some kind of game

## Proposal Document

* Your proposal document should be about 1 page. A Google Doc is recommended.
  (But please submit as a PDF!)

* List your group members.

* Abstract: Your idea in a sentence or two. New project or contribution?

* Open-source or not. If not, why?
  You don't need to pick a license right now, but Apache and MIT are good.

* Project outline: Your minimal goals, expected goals, and stretch goals.
    * This should be detailed enough that we can judge the complexity and merit.
    * Include appropriate technical details of each goal.

* Tentative Schedule: Which goals do you want to finish each week? Milestone
  presentations will be at the beginning of class each week.

## Milestone Presentations

These will be very quick (~2 min). Just update us on your progress.
You may present documentation or markdown on GitHub. Send a link before
class so we can open it on the classroom computer.

All of your work must be in your repository by class time, including
rustdoc documentation for the work you're presenting (see below).
If you're contributing, this means it should be in your fork.

## Documentation

If you are working on a standalone project, you should have rustdocs
documenting all of the work you've done so far,
at each milestone and the final presentation. This doesn't mean documenting
every single function - but you need detailed documentation for the modules,
functionality, and all important structs/functions.

Please host docs somewhere online. You can use hosting on Eniac, or you may
use a [GitHub Pages](https://pages.github.com/) project site.

By default, `cargo doc --no-deps` will export documentation for everything
_public_ in your crate. However, for the project, you'll likely want to
export everything. To do this, use:

```sh
cargo rustdoc -- --no-defaults --passes collapse-docs --passes unindent-comments
```

If you're contributing to another project, you should send us a compilation of
the documentation for everything **you** have written as part of your project.

If there is any additional extra information that you want
to send to us but think doesn't belong in
the documentation, email it to us before your presentation.

## Final Report

TBA
