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
Spend time on this and write in-depth documentation!
You'll need to use rustdoc to generate documentation before each presentation
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

**Important:** Spend time on this and write in-depth documentation.
It will be a significant part of our grading!

For any general information, put it on your crate root module. You can use
this syntax for module documentation:

```
//! if you put this documentation comment style in a
//! module, it will apply to the module itself instead
//! of the thing after it
```

Please host docs somewhere online. You can use
[hosting on Eniac](http://www.seas.upenn.edu/cets/answers/webpage.html),
or you may use a [GitHub Pages](https://pages.github.com/) project site.

By default, `cargo doc --no-deps` will export documentation for everything
_public_ in your crate. However, for the project, you'll likely want to
export everything. To do this, use:

```sh
cargo rustdoc -- --no-defaults --passes collapse-docs --passes unindent-comments
```

Here is some [example rustdoc output](http://cis198-2016s.github.io/final-sample-rustdoc/webchat/).
This shows the output of the above command on our HW07 solution. Note that it
doesn't have any _module-level_ documentation - which will be most important
for you.

If you're contributing to another project, you should send us a compilation of
the documentation for everything **you** have written as part of your project.

If there is any additional extra information that you want
to send to us but think doesn't belong in
the documentation, email it to us before your presentation.

## Final Report

Write your report in markdown, and save it as `REPORT.md` in your repository
root, and we'll find it there. If you don't have a dedicated repository for
this project (e.g. you are forking another project) or you plan to immediately
publicize your project, put the report in a [Gist](https://gist.github.com/)
and email us the link. If you have screenshots or images, save them in the repo
and embed them in `REPORT.md`.

We'll expect about 2-3 printed-pages-worth of info. Include whatever is
appropriate or important for your project, such as:

* Summary
* Approximate time spent
* Accomplishments
* Components, structure, design decisions
* Testing approach and results
* Benchmarks
* Limitations
* Postmortem
    * What went well
    * What you would do differently
* etc.

If your project would benefit from having a video, that would be great. You
can, e.g., record using [OBS](https://obsproject.com/) and upload to YouTube.

**Your documentation is still a part of the final milestone grade.** Anything
you've written for documentation can be copied to your report if it's
important. Make sure you have an appropriate amount of documentation. If you
have a ton of code but haven't written much documentation for it, your rustdoc
output will look very sparse and it'll be harder for us to tell how much you
have done. Comments don't count, as they don't appear in the rustdoc output.

Keep in mind that if your repo is public, your report will be as well.
If you have any private feedback, email us.
