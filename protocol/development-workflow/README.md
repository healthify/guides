Development Workflow
====================

Setting Up the Release Branch
-----------------------------

As a team, determine the scope of a release. The release can contain one
Pivotal Tracker story, or it can combine multiple stories that cannot be
deployed independently of one another. A release will be staged, accepted,
integrated, and deployed as a single unit.

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

Create a QA branch off of `master`.

    git checkout master
    git pull
    git checkout -b qa/<release-name>

Merge the release branch into the QA branch.

    git merge <release-name>

Deliver the QA branch to the staging app.

    git push staging head:master

Push the QA branch to GitHub.

    git push -u origin qa/<release-name>

Open a pull request to merge the QA branch into `master`,
triggering a Travis CI build to run our full test suite.

Wait for the staging delivery to finish.

Conduct QA through the [staging app front-end](https://app.healthify-staging.us).

Rejecting
---------

Close the QA branch's pull request on GitHub without merging.

Delete the QA branch on GitHub.

Rollback the new database migrations, if any, on the staging app.

    git diff master --name-only -- db/migrate
    aptible ssh --app staging-healthify bin/rake db:rollback STEP=<number-of-migrations>

Reset the staging remote.

    git checkout master
    git push -f staging master

Delete the QA branch locally.

    git branch -D qa/<release-name>

Indicate in Pivotal Tracker that the release was rejected.

Accepting and Deploying
-----------------------

Merge the pull request on GitHub for the QA branch, triggering
CI to run all tests and deploy `master` to production.

Delete the QA branch on GitHub.

Update your local `master` branch.

    git checkout master
    git pull

Delete the QA branch locally.

    git branch -d qa/<release-name>

Delete the release branches locally and remotely.

    git branch -d <release-name>
    git push origin :<release-name>

Indicate in Pivotal Tracker that the release was accepted and deployed.

If QA will not be immediately conducted on another story,
push the new `master` to the staging app.

    git push staging master

Further reading:
[A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)
[Continuous Delivery (Wikipedia)](https://en.wikipedia.org/wiki/Continuous_delivery)
