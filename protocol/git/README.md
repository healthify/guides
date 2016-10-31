# Proposal: Use release branches and cease automatic staging

Our current process for integrating, delivering, and deploying to healthify:

1. Create a changeset on a development branch forked from `master`.
2. Submit a pull request to merge the branch into `master`.
3. Review the changes in the pull request.
4. Merge the development branch into `master`, triggering delivery to staging.
5. Conduct manual QA through the front end of the staging app.

This process is conducted _concurrently_ for multiple changesets. If we have at least one changeset that has passed all 5 steps, and none that are between steps 4 and 5, we are ready to deploy.

We recognize three resulting problems:

1. We cannot deploy when there is any changeset that has finished 1-4 but not 5.
2. Sometimes, features are constituted by many distinct changesets.
3. Sometimes, a set of many features should be deployed simultaneously.

Therefore, we propose a two-part solution: Use release branches to integrate multiple dependent changesets, and decouple the post-code-review integration from the staging deployment.

1. Use release branches. Every branch to be merged into master should represent an atomically deployable feature set (i.e,. a release). Changes should be committed on _branches off of the release branch_ as opposed to branches off of master. There should be at least one development branch for each release branch; there can be multiple development branches for a release branch, to map better to Pivotal stories or to yield more digestible PRs.
2. Cease automatic staging deployment. As each component development branch is reviewed, revised, and approved it should be _merged into the release branch_. Therefore, as development progresses, the release branch will accumulate merges of approved development branches. Given a release branch, `my-release`, whose component branches have all passed review, QA may be conducted when ready, according to this console-driven process:
  1. Pull `origin/master` to the local `master`.
  2. Fork a new local `stage-my-release` branch off of `master`.
  3. Merge `origin/my-release` into `stage-my-release`.
  4. Push `stage-my-release` to `staging/master`.
  5. Wait for the staging deploy to finish.
  6. Conduct QA through the staging front-end.
  7. If QA is successful:
    1. Merge `stage-my-release` back into `master`.
    2. Push `master` to `origin/master`.
    3. Push `master` to `staging/master`.
    4. Push `master` to `production/master`, deploying it to production. (As a result, we no longer need an `origin/production` branch.)
  8. If QA is not successful:
    1. Indicate that the feature-set was rejected, in Pivotal Tracker.
    2. Push `origin/master` to `staging/master` to reset the state of staging.
  9. Delete the `stage-my-release` branch.

Therefore, our proposed development workflow:

1. Fork a release branch, `my-release`, from `master`.
2. Fork a development branch for each required component changeset of the release. There should be at least one.
3. Submit a pull request for each development branch, and review the component changeset as normal.
4. Merge the development branch **into the release branch** in GitHub, after successful code review. (This merge should **not** trigger any automatic deployment or delivery.)
5. When ready to conduct QA, fork a new `stage-my-release` branch from `origin/master`. In the console, merge `origin/my-release` into `stage-my-release` and deliver `stage-my-release` to the staging app.
6. **If QA is successful**, merge `stage-my-release` back into `origin/master`, deploy it to origin, staging, and production, and clean up your extra branches. **If QA is not successful**, reset the staging app to `origin/master` and note the rejected QA in Pivotal. See above for more details about the QA process.

Further reading:
[A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)
[Continuous Delivery (Wikipedia)](https://en.wikipedia.org/wiki/Continuous_delivery)
