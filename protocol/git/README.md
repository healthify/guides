Git Protocol
============

A guide for programming within version control.

Maintain a Repo
---------------

* Avoid including files in source control that are specific to your
  development machine or process.
* Perform work in a development branch.
* Use a [pull request] for code reviews.
* Delete local and remote development branches after merging.

[pull request]: https://help.github.com/articles/using-pull-requests/

Commit Messages
---------------

Write [good commit messages]. Example format:

    Present-tense summary under 50 characters

    * More information about commit (under 72 characters).
    * More information about commit (under 72 characters).

    http://project.management-system.com/ticket/123

[good commit messages]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
