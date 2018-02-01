# Code Review

A guide for reviewing code and having your code reviewed.

## Codebase Maintainers

### Goal

To ensure that software development has the eyes of developers that have
significant context on the Healthify system and meet HIPAA/HITRUST requirements
for secure software development lifecycles (SDLCs).

For more information, review the [ISMS policy documentation in Aptible][isms-policy-link].

### Policy

* For every Healthify codebase that contains sensitive data, one or many code
  maintainers are defined.
* A code maintainer (CM) is defined as someone that has worked at Healthify for 90
  days and has been approved by engineering leadership to be a code maintainer.
* If you are a CM:
  * When developing software you still must receive code review, but code review
    may happen by any developer, whether they are a CM or not.
* If you are not a CM:
  * When developing software, you must receive an approved code review by a CM.
  * When performing code review, you must not approve a pull request in the
    Github web UI.
    * With that said, you can (and are encouraged to) perform code review.
    * After completing your review, request review from a CM so that they can
      sign-off on the pull request.

## General Guidance

* Accept that many programming decisions are opinions. Discuss tradeoffs, which
  you prefer, and reach a resolution quickly.
* Ask questions; don't make demands. ("What do you think about naming this
  `:user_id`?")
* Ask for clarification. ("I didn't understand. Can you clarify?")
* Avoid selective ownership of code. ("mine", "not mine", "yours")
* Avoid using terms that could be seen as referring to personal traits. ("dumb",
  "stupid"). Assume everyone is attractive, intelligent, and well-meaning.
* Be explicit. Remember people don't always understand your intentions online.
* Be humble. ("I'm not sure - let's look it up.")
* Don't use hyperbole. ("always", "never", "endlessly", "nothing")
* Don't use sarcasm.
* Keep it real. If emoji, animated gifs, or humor aren't you, don't force them.
  If they are, use them with aplomb.
* Talk synchronously (e.g. chat, screensharing, in person) if there are too many
  "I didn't understand" or "Alternative solution:" comments. Post a follow-up
  comment summarizing the discussion.

## Having Your Code Reviewed

* Be grateful for the reviewer's suggestions. ("Good call. I'll make that
  change.")
* Don't take it personally. The review is of the code, not you.
* Explain why the code exists. ("It's like that because of these reasons. Would
  it be more clear if I rename this class/file/method/variable?")
* Extract some changes and refactorings into future tickets/stories.
* Link to the code review from the ticket/story. ("Ready for review:
  https://github.com/organization/project/pull/1")
* Push commits based on earlier rounds of feedback as isolated commits to the
  branch. Do not squash until the branch is ready to merge. Reviewers should be
  able to read individual updates based on their earlier feedback.
* Seek to understand the reviewer's perspective.
* Try to respond to every comment.
* Wait to merge the branch until Continuous Integration (TravisCI)
  tells you the test suite is green in the branch.
* Merge when approved by the person reviewing your code.

## Reviewing Code

Understand why the change is necessary (fixes a bug, improves the user
experience, refactors the existing code). Then:

* Communicate which ideas you feel strongly about and those you don't.
* Identify ways to simplify the code while still solving the problem.
* If discussions turn too philosophical or academic, move the discussion offline
  to a regular Friday afternoon technique discussion. In the meantime, let the
  author make the final decision on alternative implementations.
* Offer alternative implementations, but assume the author already considered
  them. ("What do you think about using a custom validator here?")
* Seek to understand the author's perspective.
* Sign off on the pull request by approving a [pull request
  review][pr-review-docs] in Github.

## Style Comments

Reviewers should comment on missed [style](../style)
guidelines. Example comment:

    [Style](../style):

    > Order resourceful routes alphabetically by name.

An example response to style comments:

    Whoops. Good catch, thanks. Fixed in a4994ec.

If you disagree with a guideline, open an issue on the guides repo rather than
debating it within the code review. In the meantime, apply the guideline.

[isms-policy-link]:https://dashboard.aptible.com/gridiron/986de3df-091e-482a-b8c3-513ddd08cf87/
[pr-review-docs]:https://help.github.com/articles/about-pull-request-reviews/
