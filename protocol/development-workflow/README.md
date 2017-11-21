# Development Workflow

## Setting Up the Release Branch

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

## Developing for the Release

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

## Conducting QA

If this is your first time conducting QA, please reference [QA Setup](qa-setup.md) in these guides.

Get the most recent state of the release branch.

    git checkout <release-name>
    git pull

Create a QA branch off of `master`.

    git checkout master
    git pull
    git checkout -b qa/<release-name>

Merge the release branch into the QA branch.

    git merge <release-name>

Copy the QA steps from the Pivotal Tracker story into the PR description, and put the story ID in the PR title. Add the link to the PT story in the description of the PR.

Determine which server is available on the [server spreadsheet](https://docs.google.com/spreadsheets/d/1qZ5x80cYXHxACJbZ20W5MqH6ETRFDmaLTnsjaqen6O0/edit), update the row of your choice, and push to that server.

    git push <APP REMOTE> head:master --force

Where <APP REMOTE> is the name of the remote you set up for the server of your choice. If you're not sure what's available, use `git remote -v` and refresh yourself by reading the [QA Setup](qa-setup.md) steps.

Push the QA branch to GitHub.

    git push -u origin qa/<release-name>

Open a pull request to merge the QA branch into `master`, triggering a Travis CI build to run our full test suite.

Wait for the staging delivery to finish.

Conduct QA through your staging app's frontend. Reference the Server Spreadsheet to find the correct URL for your app.

If required by the specified QA steps, use the staging console.

    aptible ssh --app <APP NAME> bin/rails c

### Rejecting

Close the QA branch's pull request on GitHub without merging.

Delete the QA branch on GitHub.

Rollback the new database migrations, if any, on the staging app.

    git diff master --name-only -- db/migrate
    aptible ssh --app <APP NAME> bin/rake db:rollback STEP=<number-of-migrations>

Reset the staging remote.

    git checkout master
    git push -f <APP REMOTE> master

Delete the QA branch locally.

    git branch -D qa/<release-name>

Indicate in Pivotal Tracker that the release was rejected.

Be sure to update the server spreadsheet when you are done using a staging server.

### Accepting and Deploying

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

If QA will not be immediately conducted on another story, push the new `master` to the staging app, as well as all unused staging servers.

    git push <APP REMOTE> master

Be sure to update the server spreadsheet when you are done using a staging server.

Further reading:
[A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)
[Continuous Delivery (Wikipedia)](https://en.wikipedia.org/wiki/Continuous_delivery)


## Small, discretionary releases

Sometimes a change is so small that it doesn't warrant the entire
workflow process described above. For instance, a single typo in
the translation files, or refactoring tests. In
these scenarios, the developer is welcome to make a pull request
pointed directly at master given the following:

1. The developer communicates to the code reviewer that this is
going directly into master.
2. The code reviewer agrees that it is reasonably small and agreeable
that the normal process is skipped.

Given those conditions, the following should happen:

After code review is completed, the developer manually pushes
their release branch onto a staging server.

    git push <staging-remote> head:master

> Don't forget to update the [Server Status Spreadsheet](https://docs.google.com/spreadsheets/d/1qZ5x80cYXHxACJbZ20W5MqH6ETRFDmaLTnsjaqen6O0/edit#gid=0) before and after you perform QA.

Note: In the case of a dependency update, QA involves deploying to a staging server and ensuring that the application doesn't crash.

When completed, the developer does their own QA to ensure things
are still in ship shape.

Given that it passes on staging, the developer may merge their
pull request into master.
