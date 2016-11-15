Development Workflow
====================

Setting Up the Release Branch
-----------------------------

As a team, determine the scope of a release. The release can contain one
Pivotal Tracker story, or it can combine multiple stories that cannot be
deployed independently of one another.

Create a release branch off of `master`.

    git checkout master
    git pull
    git checkout -b <release-name>

Push the release branch to GitHub.

    git push -u origin <release-name>

Developing for the Release
--------------------------

Create a development branch off of the release branch.

    git checkout <release-name>
    git pull
    git checkout -b <development-branch-name>

Commit changes.

Rebase over the release branch.

    git fetch origin
    git rebase origin/<release-name>

Push the development branch to GitHub.

    git push -u origin <development-branch-name>

Open a pull request to merge the development branch into the release branch.

Conduct Code Review.

Merge the development branch into the release branch.

Repeat this process as many times as necessary to complete all
components of the release. When the last development branch has been
merged, indicate in Pivotal Tracker that the release is delivered and
ready for QA.

Conducting QA
-------------

Get the most recent state of the release branch.

    git checkout <release-name>
    git pull

Create a staging branch off of `master`.

    git checkout master
    git pull
    git checkout -b qa/<release-name>

Merge the release branch into the release's staging branch.

    git merge <release-name>

Push the release's staging branch to GitHub.

    git push -u origin qa/<release-name>

Open a pull request to merge the release's staging branch into `master`,
triggering a Travis CI build to

    * run our full test suite, and if passing,
    * deliver the branch to our staging app.

Wait for the staging delivery to finish.

Conduct QA through the staging app front-end.

Rejecting
---------

Rollback the database migrations of this release on staging.

Reset the staging remote.

    git checkout master
    git push staging master

Delete the release's local staging branch.

    git branch -D qa/<release-name>

Close the pull request on GitHub without merging.

Delete the release's staging branch on GitHub.

Indicate in Pivotal Tracker that the release was rejected.

Accepting and Deploying
-----------------------

Merge the pull request on GitHub for the release's staging branch,
triggering a script to

    * deliver `master` to staging, and
    * deploy `master` to production.

Delete the release's staging branch on GitHub.

Update your local `master` branch.

    git checkout master
    git pull

Delete the release's local staging branch.

    git branch -d qa/<release-name>

Delete the local and remote release branches.

    git branch -d <release-name>
    git push origin :<release-name>

Indicate in Pivotal Tracker that the release was accepted and deployed.

Further reading:
[A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)
[Continuous Delivery (Wikipedia)](https://en.wikipedia.org/wiki/Continuous_delivery)
