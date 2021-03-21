
# On Git Commit Messages

When committing a change in git, you are asked to provide a commit message. Millions of developers
do this a dozen times a day. The resulting commit messages are of varying styles and quality. This
document attempts to examine what makes a good commit message and lay out a case for trying to make
a good commit messages whenever you can.

## What is a good commit message?

A good commit message is a message that provides adequate context and reasoning for the change being
made. In the case of fixing a typo, this might be `"Fixed typo"`. In the case of fixing a hard to
track down bug that has plagued the system for a long time or taken hours or days to figure out,
this might mean a full page of text with completely paragraphs and links and references to
information sources that aid in the understanding of the change being made.

The length of a good commit message is not necessarily related to the size of the code change being
committed. Some one line changes can take two paragraphs to explain. Some commits that touch a
hundred files can be mundane and obvious. Large chunks of work can be sensibly broken up into
multiple commits each with their own messages for varying length.

The important thing is that if someone looks at the change being made, any and all of their
questions should be answered by the commit message provided.

## Why

Here we attempt to document the reasons for writing good commit messages.

### Understanding

The most obvious reason for a good commit message is to help people to understand the changes being
made. This might be your colleague doing a code review the same day you made it. This might be a
team member trying to understand your change a week later when you're on holiday. This might be a
new person joining the project and trying to understand the reason for the code being in the state
that it is in. This might be you trying to understand the change six months later having been off
the project for four of those months.

Whatever the scenario, there are a few different reasons why a good commit message might provide
important context for the state or change of the code in your codebase.



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

## Personal History

## References

- [A Note About Git Commit Messages - tbaggery.com](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) - 19 April 2008
