
# On Git Commit Messages

When committing a change in git, you are asked to provide a commit message. Millions of developers
do this a dozen times a day. The resulting commit messages are of varying styles and quality. This
document attempts to examine what makes a good commit message and lay out a case for trying to make
a good commit messages whenever you can.

## What is a good commit message?

A good commit message is a message that provides adequate context and reasoning for the change being
made. In the case of fixing a typo, this might be `"Fixed typo"` as the change is genuinely self
explanatory. In the case of fixing a hard to track down bug that has plagued the system for a long
time or taken hours or days to figure out, this might mean a full page of text with complete
paragraphs, links and references to information sources that aid in the understanding of the change
being made.

The length of a good commit message is not necessarily related to the size of the code change being
committed. Some one line changes can take multiple paragraphs to explain. Some commits that touch a
hundred files can be mundane and obvious and require only a sentence or two. Large chunks of work
can be sensibly broken up into multiple commits each with their own messages for varying length.

The important thing is that, if someone looks at the change being made, ideally any and all of their
questions should be answered by the commit message provided.

## Why

Here we attempt to document the reasons for writing good commit messages.

### Understanding

The most obvious reason for a good commit message is to help people to understand the changes being
made. This might be:

- Your colleague doing a code review the same day you made it.
- A team member trying to understand your change a week later when you're on holiday
- A new person joining the project and trying to understand the reason for the code being in the
  state that it is in.
- You trying to understand your own changes six months later having been off the project for four of
  those months.

Whatever the scenario, there are a few different reasons why a good commit message might provide
important context for the state or change of the code in your codebase.

What kind of understanding do we want from a commit message? Mostly we want to know why the change
was made and what assumptions or knowledge the committer had when making the change. This helps
future developers to understand the reasons behind the code and whether they were well founded.


### Reflecting on your Changes

Writing a clear and comprehensive commit message can act as a opportunity to reflect on the changes
that you are making to make sure that they can be explained and to document any part that can't.

Ideally whilst writing a commit message, you have a view of the to-be-committed changes available to
you. You can scroll through the diff and make sure that you understand and are happy with the
changes you see and that your commit message covers anything that might not be completely clear.
This might include references to source, eg. StackOverflow questions & answers, that have helped you
come to the conclusion and the set of changes that you have made.

This process can be used to hold yourself to account. You should explain any changes that you have
made that you don't fully understand but appear to work as that allows future developers to
understand your state of mind and doesn't lend false authority to the changes.

You can also explain things that you feel could be better but that you did not have time to add for
various reasons. Sometimes I can use this to guilt myself into making changes that I know I should
do. I can sometimes make less than ideal changes but I find it hard to write a commit message
explaining that I know they are less than ideal and so end up doing the improvements to get them to
where I think they should be.


### Knowledge Transfer

This is part of 'understanding' in many ways but beyond the 'in the moment' comprehension of what is
going on in the commit, a good commit message can share situational understanding, communicate
prioritisation of goals, explain oddities in tooling or languages, highlight unexpected user
behaviours and other things that provide context to the project.

Are git commit message the best way of transferring this kind of knowledge? No, but they are a
situational advantage of being attached to the code and change that someone is currently examining.


## Structure

The structure of the commit message is important but not at important as the content. There are a
few rules to follow when creating a commit message:

1. The first line should not exceed 50 characters
2. The second line should be blank.
3. The rest of the lines in the commit messages should not exceed 72 characters.

These are not hard rules but rather conventions that are useful to follow as tooling in the git
ecosystem can rely on them. The first line is considered to be the subject of the commit message,
like an email has a subject line. The second line separates the subject from the body. The rest is
the body.

Personally, I think of the limits as soft limits. If I have to sacrifice clarity to get the subject
line to 50 characters then I'll happily use 55 characters instead. If I'm pasting in example code or
a URL to the commit message body then I won't worry if those lines exceed 72 characters.

Due to the expectation that the body text is wrapped to 72 characters, most user interfaces will not
attempt to wrap the content of the commit messages themselves. If they do, it can interfer with user
formatting.

## Creating Commit Messages 

Here we explore ways of creating commit messages and the various pros and cons of these methods.

### Command Line Flags

You can add a commit message to a commit using the `-m` flag. As follows:

  ```shell
  git commit -m "Support parsing operators in 'exposing' lists"
  ```

This quick but only creates a one line commit message which is unlikely to be sufficient. We can
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

### Integrated User Interfaces

## Examples

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
  how to make an edit to one of these and one of the steps was to enter '.' at the prompt. It took
  me a while to realise that that was the commit message prompt. Enter '.' to make it go away was a
  serious as we took commit messages in that situation.

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
in commit messags and comments, that later turn out to be incrediblity obvious to anyone familiar
with the technology.

I don't know where the line exists for this. It is often best to think about the average or lower
levels of experience for people in your team and write for them. If you only write for highly
experience developers then you message might be opaque to more junior members but at the same time
you don't want to be explaining basic language constructs or the standard library in commit
messages.
