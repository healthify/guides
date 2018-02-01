# Development Workflow

People: Developer, Code Reviewer, QA-er

Tools: Github (GH), Pivotal Tracker (PT), Aptible Staging Servers

## Developer
1. Create a `release/` branch off the latest version of `master` and push to GH

    a. `git pull` for the latest state of `master`
    
    b. *Start* the PT story<sup>[0](#fn0)</sup>

2. Create a `dev/` branch off that release branch and push to GH<sup>[1](#fn1)</sup>

3. Develop the story, committing and pushing to GH along the way

4. When finished, open a PR of the `dev/`  into the `release/` with the `needs review` label
    
    a. *Finish* the PT story (assuming no more future `dev/` branches for the `release/`)<sup>[2](#fn2)</sup>
    
    b. Solicit Code Review

5. Upon getting GH Approval from the Code Reviewer and making final revisions, merge the PR;
    
    a. If the `release/` branch is not up-to-date with `master`, update it<sup>[3](#fn3)</sup>

    b. *Deliver* the PT story and add QA Steps (including `release/` name); Solicit QA

## QA-er
1. Pull down the appropriate `release/` branch and indicate which QA server you're taking

    a. If the `release/` branch is not up-to-date with `master`, update it<sup>[3](#fn3)</sup>

    b. Label the server via the [Server Spreadsheet](https://docs.google.com/spreadsheets/d/1qZ5x80cYXHxACJbZ20W5MqH6ETRFDmaLTnsjaqen6O0/edit)

2. Push the updated `release/` to your QA server and to GH

3. Open a PR of the `release/`  into the `master` with the `needs QA` label

4. Perform your QA, leaving some summary notes in comments on the PR

5. **If QA passes**, merge the PR.
    
    a. *Accept* the PT story, with a comment linking to the PR
    
    b. Clear out your name from the Server Spreadsheet and push the latest `master` to
    all unused QA servers
   
   **If QA fails**, document why on the PR and close the PR without merging.
    
    a. *Reject* the PT story, with a comment linking to the PR
    
    b. Clear out your name from the Server Spreadsheet and push the latest `master` to 
    QA server you were using


## Footnotes
<a name="fn0">0</a>: You can do this like a pro by leveraging the [GH-PT integration](https://content.pivotal.io/blog/level-up-your-development-workflow-with-github-pivotal-tracker)

<a name="fn1">1</a>: Developers can use their discretion to sometimes skip creating `dev/` branches, going
straight to a *discretionary release branch* (e.g for a typo in the translation files or a tests
refactoring). When opting for this approach, sanity check with another [Codebase Maintainer](https://github.com/healthify/guides/tree/master/code-review)
that the discretionary release makes sense in your case. In these cases, neither create a `dev/`
branch nor merge your PR after CR, since you'll be doing CR and QA both using the same GH PR. Notably,
for very small changes (or dependency updates), often the only QA needed is to sign in to the app and
ensure neither the deploy nor the app crashed.

<a name="fn2">2</a>: Although many stories may require just one `dev/` branch per `release/`, some
stories are sufficiently large that breaking them into multiple `dev/` branches that each get their own
GH PR makes things easier to develop and especially to Code Review. PRs that get north of 250 lines
of diff can compel either overly onerous or overly hasty Code Review.

<a name="fn3">3</a>: You can use `git merge` or `git rebase`. Merges are often nice when you must resolve merge conflicts;
    rebases are often nice otherwise when you want to avoid adding a merge commit
