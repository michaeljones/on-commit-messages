
# On Git Commit Messages

When committing a change in git, you are asked to provide a commit message. Millions of developers
do this a dozen times a day. The resulting commit messages are of varying styles and quality.

## Goal

Instead of writing commit messages like:

    Fix mobile width issue

We want to write commit messages like:

    Fix mobile width issue

    It seems that the various 'edit' links under the headers were right
    aligned and with some margin or negative margin (from the 'row' class)
    that was causing it to push out beyond the edge of the page on mobile
    leading to a weird space down the right.

    This seems to address it and uses flexbox instead of float right, I
    suspect the first pass of this site didn't use flexbox for fear of
    compatibility issues but we can certainly use it now.

It is perfect? No, but it provides much more context, explanation and reasoning.

## What is a good commit message?

A good commit message is a message that provides adequate context and reasoning for the change being
made.

In the case of fixing a typo, this might be `Fixed typo` as the change is genuinely self
explanatory. In the case of fixing a hard to track down bug, this might mean a half page of text with
paragraphs, links and references to information that aid in the understanding of the change
being made.

The length of a good commit message is not necessarily related to the size of the diff. Some one
line changes take multiple paragraphs to explain. Some commits that touch a
hundred files can be mundane and require only a sentence or two.

The important thing is that any and all questions about the diff should be answered by the commit
message provided.

## Why

*The reasons for writing good commit messages.*

### Understanding

The best reason for good commit message is to help people understand the changes being made. This
might be:

- Your colleague doing a code review the same day you made it.
- A team member trying to understand your change a week later when you're on holiday.
- A new person joining the project and trying to understand the reason for the code being in the
  state that it is in.
- You trying to understand your own changes six months later.

What kind of understanding do we want from a commit message? Mostly we want to know why the change
was made and what assumptions or knowledge the committer had when making the change. This helps
future developers to understand the reasons behind the changes and whether they were well founded.


### Reflecting on your Changes

Writing a clear and comprehensive commit message can act as a opportunity to reflect on the changes
that you are making to make sure that they can be explained and to document any part that can't. It
acts as a mini code review.

Ideally whilst writing a commit message, you have a view of the to-be-committed changes available to
you. You can make sure that you understand and are happy with the
changes you see and that your commit message covers anything that might not be clear.
This might include references to sources of information, eg. StackOverflow questions & answers, that
have helped you come to the set of changes that you have made.

This also helps you to review the changes themselves. Maybe you left in
some print-statements, maybe a section of the diff can be handled in a separate commit. This stage
of reviewing allows you to spot these issues and clean up your commits in advance.

This process can also be used to hold yourself to account. You should explain any changes that you have
made that you don't fully understand but that appear to work as that allows future developers to
understand your state of mind and doesn't lend false authority to the changes. You can also explain
things that you feel could be better but that you did not have time to add for
various reasons. [1](#foot-note-guilt)


## Structure

The structure of the commit message is important but not as important as the content. There are a
few rules to follow when creating a git commit message:

1. The first line should not exceed 50 characters
2. The second line should be blank.
3. The rest of the lines in the commit messages should not exceed 72 characters.

These are not hard rules but rather conventions that are useful to follow as tooling in the git
ecosystem can rely on them. The first line is considered to be the subject of the commit message,
like an email has a subject line. The second line separates the subject from the body. The rest is
the body.

Personally, I think of the limits as soft limits. If I have to sacrifice clarity to get the subject
line to 50 characters then I'll happily use 55-60 characters instead. If I'm pasting in example code
or a URL to the commit message body then I won't worry if those lines exceed 72 characters.

## Creating Commit Messages 

*Explore the various ways of creating a commit message.*

### Command Line Flags

You can add a commit message to a commit using the `-m` flag. As follows:

  ```shell
  git commit -m "Support parsing operators in 'exposing' lists"
  ```

This is quick but only creates a one line commit message which is unlikely to be sufficient. We can
provide more lines by specifying the `-m` flag multiple times. The `git commit` command joins the
various `-m` messages together with a blank line between each.

  ```shell
  git commit -m "Support parsing operators in 'exposing' lists" -m "So that it is easier to retrieve the implementation of the operator when checking & evaluating it. Previously when looking up the 'add' function associated with the '+' operator it would find the 'add' function in the scope of the current module and not the one in the module that defined '+'." -m "This isn't the best fix as it doesn't generalise to other functions that call functions but it seems reasonable to do in the case of operators."
  ```

This is clearly awkward to write on a command line and the command does not wrap long lines so the
result is:

  ```
  Support parsing operators in 'exposing' lists

  So that it is easier to retrieve the implementation of the operator when checking & evaluating it. Previously when looking up the 'add' function associated with the '+' operator it would find the 'add' function in the scope of the current module and not the one in the module that defined '+'.

  This isn't the best fix as it doesn't generalise to other functions that call functions but it seems reasonable to do in the case of operators.
  ```

Which is not that readable. The approach also makes it hard to introduce formatted lists or blocks
of code.

This is not a recommended approach.

### Command Line Editors

If you run `git commit` without the `-m` flag then git will attempt open your editor of choice for
you to compose your commit message. In order to figure out what editor to use, it checks, in order:

- `GIT_EDITOR` environment variable
- `core.editor` config value
- `VISUAL` environment variable
- `EDITOR` environment variable

And if none of those are set then it uses `vi` on your system. If you are not used to it, `vi` is a
confusing and complicated experience that can be off-putting. [2](#foot-note-vi) Good alternatives
to `vi` for composing your commit messages are `nano`, `pico` and `micro` depending on what your
system has installed. Nano includes shortcut help at the bottom of the display to help you figure
out how to save and exit.

Well configured command line editors will provide you with syntax highlighting and automatic line
wrapping to help you keep to the appropriate structure of a commit message without confining you to
it.

It is possible to use non-command line based editors provided they can be launched in a
"blocking" fashion so that the `git commit` command can assume that the commit message will have
been completed by the time your editor closes. An example of this is running `gvim` with the `-f`
(foreground) flag. It is harder to use IDE-like editors in this role as they don't tend to be opened
and closed on a per-file basis.

#### Setting your Command Line Editor

If you are unfamiliar with shells and their configuration, then the easiest approach is probably the
`core.editor` git config value. You can set it with:

```
git config --global core.editor nano
```

If you are familiar with your `.bashrc` or `.zshrc`, you can add a line setting the environment
variable of your choice:

```
export EDITOR=nano
```

If you want to play around with options and different editors then you can set it as part of the
command itself:

```
GIT_EDITOR=nano git commit
```

### Integrated User Interfaces

There are a number of editors, IDEs and git tools that allow you to compose commit messages inside
them and commit directly from their interface. These are great provided they encourage multiline
commit messages and give guidance and help in adhering to the desired structure.

### Visual Studio Code

VSCode has a git integration and includes a box for composing commit messages in the top left of the
git tooling. The box appears as a single line but will expand to accommodate new lines in the
message to allow for a multiline commit message.

It warns when the first line exceeds 50 characters and when other lines exceed 72 characters.

Unfortunately by default the box is too small. It is part of the left hand side bar and so normally does not have room for a 72
character wide message. It wraps lines within the set width of the box but does not introduce line
breaks for you. This can lead to visually confusing results where, on the third line of a message in
a box that fits 30 characters a line, you start to get warnings that your line is over 72
characters.

You can improve the experience a little by increasing the width of the side bar when creating commit
messages so that there is room for 72 characters.

It does not automatically add line breaks for your at the character limits so you have to manually
add line breaks when you go over the limits.


## Types of Commit Message

There are only so many types of changes that you might make to a code base and there is a reasonable
commit message style to match each one.

### Additional Feature

- Any reasonably large feature will break down into a series of commits so there rarely a single
  commit that has a whole feature in it.

- Features are introduced due to user or business requirements which don't generally need to be
  explained in the commit message.

- You might consider explaining:

  - Why were particular technology or data structure choices made? Why are we using a queue in the
    messaging system? What was the motivation to store values in a set over an array?

  - What is missing from the current implementation that you'd like to highlight? Does this first
    implementation include adequate error messages for the user? Does it have an accessible UI?

```
Add initial 'frequency' page

We display a list of the user's non-regular friends along with their
contact-periods and a live updating value of 'yearly-contacts' at the
top to allow users to adjust the numbers to reach an acceptable level of
commmitment.

Missing form submission which would allow for permanent changes. Also
missing general styling.
```

### Configuration Change

Configuration changes are often small and have large impacts. Consider explaining what drove the
choice to change the configuration. What was happening before? What should we expect now? Was any
other work required?

```
Upgrade to postgres 12

Quite a big jump but I've not updated the database ever so this is where
we get to. Django had started using some syntax for its migrations (I
think) that was only supported in 9.4 so some migrations hadn't run in
production which was an issue.

The upgrade was down with the help of:

  https://github.com/tianon/docker-postgres-upgrade

I made a copy of the underlying postgres data folder and then upgraded
that to 12 using the helper above and then changed the postgres image
version and pointed at the new data folder and redeployed.

So that this point these changes have already been deployed but they
seem to be stable and working.
```

### Dependency Update

Sometimes we update dependencies simply so that we stay up to date. Sometimes we want a particular
bugfix. Sometimes the update introduces a new feature that we could take advantage of. Have you
checked the change logs for all the changes? Are you assuming that they'll be fine? It is ok to not
read all the change logs but it is relevant context for anyone trying to solve a new bug that seems
to have appeared in the system.

```
Updated dependencies with `mix deps.update --all`

I have no read any change logs. The tests still pass so I assume it
is fine.
```

Or:

```
Update markdown renderer to 3.2.3

As this resolves the issue we've been seeing with bold links being
rendered incorrectly.
```

Or:

```
Updated Phoenix Live View to 0.13.0

And followed the instructions in the changelog to update our system
for the evolving API. Details here:

  https://github.com/phoenixframework/phoenix_live_view/blob/master/CHANGELOG.md#0130-2020-05-21
```

### Complex Bugfix

Think about documenting what problem you were seeing. What resources you have read. What approaches
you have tried. Link out to blog posts, or official documentation, or to StackOverflow answers if
they are driving your decision making.

### Trivial Bugfix

Some bugfixes are simple logic errors and seem self-explanatory in the moment but still provide some
detail.

```
Fix off by one error

To stop us missing the last element in the array.
```

Where possible trivial fixes with no story of their own should be squashed back into the commit
that introduced them provided that commit has not been shared with others.

### Trivial Rename

```
Rename Model to StaffModel

When we started out, we thought we'd be storing all the state for the
application here but we need to introduce more non-staff functionality
so we're renaming to make space for that.
```

## References

- [A Note About Git Commit Messages - tbaggery.com](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) - 19 April 2008

## Footnotes

<span name="foot-note-guilt">1</span> Sometimes I can use this to guilt myself into making changes that I know I should
do. I can sometimes make less than ideal changes but I find it hard to write a commit message
explaining that I know they are less than ideal and so end up doing the improvements to get them to
where I think they should be.

<span name="foot-note-vi">2</span> Defaulting to `vi` probably drives people towards using the `-m` flag and results in more single line commit messages.

## Appendix

### Comparison with Code Comments

It is reasonable to ask what belongs in a commit message and what belongs in comments in the code.
There are subtleties to this but normally commit messages talk about why a change is being made and
code comments talk about why the code does what it is doing.

More explicitly, commit messages are 'in the moment of the change' whilst code comments are statically bound to
the state
of the code as it stands. Commit messages will commonly be discussing what has happened previously
and why that has motivated the change being made. Code comments tend to explain particularly
confusing or complex sections of code without reference to their previous state. A comment would
only discuss a previous state of the code if it is confusing why the problem is no longer
approached in that manner.

You might refer to a comment in your commit message. If the code needs explanation in its current
state, then write that as a comment in the code and refer to it in the commit message if needed. It
does not need to be duplicated into the commit message.

There is room to go into more detail commit messages as they are stored within the repository and
not within the files. Code comments should be thorough but there isn't room to document the history
of the code base and every decision that has impacted it in comments.

### Target Audience

How much you write in a commit message often feels quite dependent on the intended audience. I find
this particularly when interacting with new technology or new languages. I tend to write explainers
in commit messages and comments, that later turn out to be incrediblity obvious to anyone familiar
with the technology. It is hard to tell at the time though.

I don't know where the line exists for this. It is often best to think about the average or lower
levels of experience for people in your team and write for them. If you only write for highly
experience developers then you message might be opaque to more junior members but at the same time
you don't want to be explaining basic language constructs or the standard library in commit
messages.

As the team grows in experience, information that might seem necessary in a commit message one month
might not seem so necessary a few months later. Always try to be wary that new people might join
your team but ultimately there are other more preferrable ways to catch them up with the basics if
needed.
