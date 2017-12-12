# Development Workflow

- [Setting Up the Release Branch](#setting-up-the-release-branch)
- [Developing for the Release](#developing-for-the-release)
- [Conducting QA](#conducting-qa)
  + [Rejecting](#rejecting)
  + [Accepting and Deploying](#accepting-and-deploying)
- [Small, discretionary releases](#small-discretionary-releases)

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

Open a pull request to merge the release branch into `master`, triggering a Travis CI build to run our full test suite.

Copy the QA steps from the Pivotal Tracker story into the PR description, and put the story ID in the PR title. Add the link to the PT story in the description of the PR.

**Note**: After creating the PR, Github will let you know whether your release branch is up-to-date with master. If not, please use `git rebase` instead of the update feature on Github (which creates a merge commit).

Wait for Travis CI to finish its build on the pull request.

Determine which server is available on the [server spreadsheet][server-spreadsheet]. Update the row of your choice, and push to that server.

    git push <APP REMOTE> head:master --force

Where `<APP REMOTE>` is the name of the remote you set up for the server of your choice. If you're not sure what's available, use `git remote -v` and refresh yourself by reading the [QA Setup](qa-setup.md) steps.

Follow the QA steps from Pivotal Tracker, conducting QA through your staging app's frontend, as needed. Reference the [Server Spreadsheet][server-spreadsheet] to find the correct URL for your app.

If required by the specified QA steps, use the staging console.

    aptible ssh --app <APP NAME> bin/rails c

Leave comments on the QA pull request as necessary to detail the QA server that was used and steps performed in the QA.

### Rejecting

Leave a comment on the QA pull request with reasons why the story did not pass QA.

Close the QA pull request on GitHub without merging.

Indicate in Pivotal Tracker that the release was rejected. Leave a comment linking to the one made in Github describing why QA did not pass.

Do not delete the release branch.

Rollback the new database migrations, if any, on the staging app.

    git diff master --name-only -- db/migrate
    aptible ssh --app <APP NAME> bin/rake db:rollback STEP=<number-of-migrations>

Reset the staging remote.

    git checkout master
    git push -f <APP REMOTE> master

Be sure to update the [server spreadsheet](server-spreadsheet) when you are done using a staging server.

When QA is ready to be performed again, make sure the release branch has been updated and re-open the original QA PR. Modify the PR description as needed.

### Accepting and Deploying

Leave a comment on the QA pull request explaining that the release has been accepted and will be deployed.

Merge the QA pull request on GitHub, triggering CI to run all tests and deploy `master` to production.

Indicate in Pivotal Tracker that the release was accepted. Leave a comment linking to the one made in Github explaining that  QA passed.

Delete the release branch both locally and remotely.

    git branch -d <release-name>
    git push origin :<release-name>

Update your local `master` branch.

    git checkout master
    git pull

If QA will not be immediately conducted on another story, push the new `master` to the staging app, as well as all unused staging servers.

    git push <APP REMOTE> master

Be sure to update the [server spreadsheet][server-spreadsheet] when you are done using a staging server.

Further reading:

- [A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)
- [Continuous Delivery (Wikipedia)](https://en.wikipedia.org/wiki/Continuous_delivery)

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

> Don't forget to update the [Server Status Spreadsheet][server-spreadsheet] before and after you perform QA.

Note: In the case of a dependency update, QA involves deploying to a staging server and ensuring that the application doesn't crash.

When completed, the developer does their own QA to ensure things
are still in ship shape.

Given that it passes on staging, the developer may merge their
pull request into master.

[server-spreadsheet]: https://docs.google.com/spreadsheets/d/1qZ5x80cYXHxACJbZ20W5MqH6ETRFDmaLTnsjaqen6O0/edit
