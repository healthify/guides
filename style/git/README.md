# Git

As with all things, try to be consistent with your Git usage.

- [Commit Messages](#commit-messages)
- [Rebasing](#rebasing)
- [Make small PRs](#make-small-prs)

## Commit messages

Write a [good commit message][good-commit-message].

Here's one example:

```
Present-tense summary under 50 characters

- More information about commit (under 72 characters).
- More information about commit (under 72 characters).

[Finishes #PT_STORY_ID]
```

- `[Finishes #PT_STORY_ID]` hooks into a Pivotal Tracker (PT) Git integration. It will automatically finish the relevant PT story, and link the commit to that story, which greatly facilitates QA and debugging.
- The `vim` text editor has syntax highlighting that greatly facilitates writing short and well-formatted Git commit messages.

When it doubt, over-communicate in your commit messages. It might seem like a waste of time now, but explaining _why_ you made a change is incredibly useful when reading legacy code. An example of that:

```
Present-tense summary under 50 characters

Problem:

Here's the problem.
Here's the problem.
Here's the problem.

Solution:

Here's the solution.
Here's the solution.
Here's the solution.

[Finishes #PT_STORY_ID]
```

## Rebasing

To ease the process of walking through Git history when debugging (ie: if you've created more than one commit), use [git rebase interactively][git-rebase-interactively] to squash them into cohesive commits with good messages.

Some examples of rebase usage:

```
git rebase -i origin/master
git rebase -i HEAD~5
git rebase -i release/feature-work
```

As with everything, use your best judgement to rebase. Here are some example cases:

- You may want to `fixup` or `squash` commits if you are correcting typos or making small fixes to a single class frequently.
- You may want to use `reword` if you prefixed a commit message with `WIP`, when it is actually an isolated change.
- Or, if you are refactoring a class, you may want that in its own commit, avoiding the rebase method altogether!

Generally speaking, you should aim to rebase and squash commits down to cohesive groups of code changes. Ensure each commit includes stable, working code.

## Make small PRs

PRs with over 250 lines of additive diff are considered large. _It is valuable to artificially break up PRs for the sake of making them smaller._ With that said, always try to make PRs deployable on their own (i.e. introduce a feature flag if needed). The smaller the PR, the easier it is to review, the faster it is to get it deployed.


[good-commit-message]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
[git-rebase-interactively]: https://help.github.com/articles/about-git-rebase/
