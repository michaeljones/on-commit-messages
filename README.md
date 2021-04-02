
# On Git Commit Messages

When committing a change in git, you are asked to provide a commit message. Millions of developers
do this a dozen times a day. The resulting commit messages are of varying styles and quality. This
document attempts to examine what makes a good commit message and lay out a case for trying to make
a good commit messages whenever you can.

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
made. In the case of fixing a typo, this might be `"Fixed typo"` as the change is genuinely self
explanatory. In the case of fixing a hard to track down bug that has plagued the system for a long
time or taken hours or days to figure out, this might mean a full page of text with complete
paragraphs, links and references to information sources that aid in the understanding of the change
being made.

The length of a good commit message is not necessarily related to the size of the code change being
committed. Some one line changes can take multiple paragraphs to explain. Some commits that touch a
hundred files can be mundane and obvious and require only a sentence or two.

The important thing is that, if someone looks at the change being made, ideally any and all of their
questions should be answered by the commit message provided.

## Why

*The reasons for writing good commit messages.*

### Understanding

The most obvious reason for a good commit message is to help people to understand the changes being
made. This might be:

- Your colleague doing a code review the same day you made it.
- A team member trying to understand your change a week later when you're on holiday
- A new person joining the project and trying to understand the reason for the code being in the
  state that it is in.
- You trying to understand your own changes six months.

What kind of understanding do we want from a commit message? Mostly we want to know why the change
was made and what assumptions or knowledge the committer had when making the change. This helps
future developers to understand the reasons behind the changes and whether they were well founded.


### Reflecting on your Changes

Writing a clear and comprehensive commit message can act as a opportunity to reflect on the changes
that you are making to make sure that they can be explained and to document any part that can't. It
acts as a mini code review.

Ideally whilst writing a commit message, you have a view of the to-be-committed changes available to
you. You can scroll through the diff and make sure that you understand and are happy with the
changes you see and that your commit message covers anything that might not be completely clear.
This might include references to sources of information, eg. StackOverflow questions & answers, that
have helped you come to the conclusion and the set of changes that you have made.

This has the additional advantage of helping you to review the changes themselves. Maybe you left in
some print-statements, maybe a section of the diff can be handled in a separate commit. This stage
of reviewing allows you to spot these issues and clean up your commits in advance.

This process can also be used to hold yourself to account. You should explain any changes that you have
made that you don't fully understand but that appear to work as that allows future developers to
understand your state of mind and doesn't lend false authority to the changes.

You can also explain things that you feel could be better but that you did not have time to add for
various reasons. Sometimes I can use this to guilt myself into making changes that I know I should
do. I can sometimes make less than ideal changes but I find it hard to write a commit message
explaining that I know they are less than ideal and so end up doing the improvements to get them to
where I think they should be.


### Knowledge Transfer

This is part of 'understanding' in many ways but goes beyond the 'in the moment' comprehension of what is
going on in the commit, a good commit message can share situational understanding, communicate
prioritisation of goals, explain oddities in tooling or languages, highlight unexpected user
behaviours and other things that provide context to the project.

Are git commit message the best way of transferring this kind of knowledge? No, but they have the
situational advantage of being attached to the code and change that someone is currently examining.


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

Due to the expectation that the body text is wrapped to 72 characters, most user interfaces will not
attempt to wrap the content of the commit messages themselves. If they do, it can interfere with
user formatting. As they don't, it is best to wrap them yourself, or ideally have your editor do it,
to avoid unreadably long lines.

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

Which is not optimally readable and this situation would be much trickier if you want to represent
a list of items or blocks of code.

This is not a recommended approach.

### Command Line Editors

If you run `git commit` without the `-m` flag then git will attempt to use your editor of choice to
open a text file for you to compose your commit message. In order to figure out what editor to use,
it checks, in order:

- `GIT_EDITOR` environment variable
- `core.editor` config value
- `VISUAL` environment variable
- `EDITOR` environment variable

And if none of those are set then it uses `vi` on your system. If you are not used to it, `vi` is a
confusing and complicated experience that can be off putting. Defaulting to `vi` probably drives
people towards using the `-m` flag and results in poorer commit messages.

Good alternatives to `vi` for composing your commit messages are `nano`, `pico` and `micro`
depending on what your system has installed. Nano includes shortcut help at the bottom of the
display to help you figure out how to save and exit.

Well configured command line editors will provide you with syntax highlighting and automatic line
wrapping to help you keep to the appropriate structure of a commit message without confining you to
it.

It is possible to use non-command line based editors provided they can be launched in a
"blocking" fashion so that the `git commit` command can assume that the commit message will have
been completed by the time your editor closes. An example of this is running `gvim` with the `-f`
(foreground) flag. It is harder to use IDE-like editors in this role as they don't tend to be opened
and closed on a per-file basis.

### Integrated User Interfaces

There are number of editors, IDEs and git tools that allow you to compose commit messages inside
them and commit directly from their interface. These are great provided they encourage multiline
commit messages and give guidance and help in adhering to the desired structure.

### Visual Studio Code

VSCode has git integration and includes a box for composing commit messages in the top left of the
git tooling. The box appears as a single line but will expand to accommodate new lines in the
message to allow for a multiline commit message.

It warns when the first line exceeds 50 characters and when other lines exceed 72 characters.

Unfortunately it is part of the left hand side bar and so normally does not have room for a 72
character wide message. It wraps lines within the set width of the box but does not introduce line
breaks for you. This can lead to visually confusing results where, on the third line of a message in
a box that fits 30 characters a line, you start to get warnings that your line is over 72
characters.

You can improve the experience a little by increasing the width of the side bar when creating commit
messages so that there is room for 72 characters.


## Types of Commit Message

There are only so many types of changes that you might make to a code base and there is a reasonable
commit message style to match each one.

### Additional Feature

### Configuration Change

### Dependency Update

### Complex Bug Fix

### Trivial Bug Fix

### Trivial Rename

## References

- [A Note About Git Commit Messages - tbaggery.com](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) - 19 April 2008

## Appendix

### Personal History

- When I started developing I used to try to write as succinct commit messages as possible. If I
  could come up with 5-8 words that felt like the covered the contents of the commit, I would pat
  myself on the back. Job well done. I don't know why I felt like this.

  A few years in, I realise these messages could be as long as you liked and I couldn't see a
  downside in just pouring out all the context I could think of into those messages. I still can't
  see one.

- I worked at a company once that kept some files in RCS or some other basic source control system.
  They were config files that barely needed editing. At some point I received some instructions on
  how to make an edit to one of these and one of the steps was to pres '.' and then 'enter' at the
  prompt. It took me a while to realise that that was the commit message prompt. Enter '.' to make
  it go away was a serious as we took commit messages in that situation.

- At another company, a colleague returned from holiday and asked what had changed on the project in
  his absence. I suggested that he could read the commit messages of the commits that had occurred.
  There were not so many. The suggestion was met with laughter which needless to say made me a
  little sad but I suspect that laughter would be a common reaction in our industry.


### Comparison with Code Comments

It is reasonable to ask what belongs in a commit message and what belongs in comments in the code.
There are subtleties to this but normally commit messages talk about why a change is being made and
code comments talk about why the code does what it is doing.

More explicitly, commit messages are temporal whilst code comments are statically bound to the state
of the code as it stands. Commit messages will commonly be discussing what has happened previously
and why that has motivated the change being made. Code comments tend to explain particularly
confusing or complex sections of code without reference to their previous state. A comment would
only discuss a previous state of the code if it is confusing whilst the problem is no longer
approached in that manner.

You might refer to a comment in your commit message. If the code needs explanation in its current
state, then write that as a comment in the code and refer to it in the commit message if needed. It
does not need to be duplicated into the commit message.

There is room to go into more detail commit messages as they are stored within the repository and
not within the files. Code comments should be thorough but there isn't room to document the history
of the code base and every decision that has impacted it in comments.

One aspect where I think they overlap more is in parts of the code that are touched less often. The
less often code is touched, the less familiar it is to the current developers and the more it can be
sensible to have lengthly comments about what is going on. I find this tends to impact 'write and
forget' config files more than code itself. A config file might remain untouched for a year and then
need a tweak and often the impacts of the small change are quite large or driven by particular
choices and it is good to document those within the file itself as that is even more available to
the next develop than the commit message in which the change happened.


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
