Database Scripting
==============

A protocol for running console scripts on the Healthify app's production database.

Development
-----------

Develop the scripting story similarly to any other story. In this case, this would
entail running your script at the rails console of your local server.

While some scripts only lend themselves to being performed via pure SQL, where it
is possible, you should leverage ActiveRecord to perform our `INSERT`/`UPDATE`/`DELETE`
operations so that we leverage ActiveRecord validations and callbacks.

While developing the script, it is critical to plan our remediation strategy if
something does go awry.

One useful approach to consider is to wrap the `INSERT`/`UPDATE`/`DELETE` commands within
a transaction, and then to assert on certain states of the database after the script
is complete, rolling back all changes if the assertions fail.

Further, it's important to be able to execute a complete reversal of your changes
in the event that there are unexpected failures. Ideally, we would write the commands
we would need to immediately reverse our changes if necessary (i.e. a reversal script).

For `INSERT` operations, we would need to retain data about what records ended up
being `INSERT`-ed by our script, and then the reversal script would delete those
records.

For `UPDATE`/`DELETE` operations, we would want to dump the data of all the records
we will be interacting with into a "rescue table" (a table of the which we would keep
for only around a week before deleting. Such a table would ideally live in a
`rescue_table.*` DB schema). Our reversal script would then perform a COPY of the data
from the rescue table into the original table (and a deletion of updated records in the
case of an Update). Although an inferior option, it alternatively often suffices to dump
the data into a csv in lieu of a rescue table.

**Handling PHI**

To remain HIPAA compliant, when our scripts touch data that is PHI, it is critically
important that we do not perform a dump of sensitive data into an insecure format.
Thus, for PHI, do not dump the data into a csv or other format, leveraging instead
a rescue table in Aptible.

Code Review
-----------

Typically, console scripts are not code that we want to add to the Healthify app's
code repo. As a result, to solicit Code Review you can put your code in a Github
[gist](https://help.github.com/articles/about-gists/) and share the gist's link
with your teammates to get Code Review. Also, be sure to include make the gist `private`
and to include a link to the gist in the description of the corresponding Pivotal Story.

Running database scripts poses a higher risk than deploying a normal feature. Hence,
it's important to have a higher bar of Code Review than for normal stories. Make sure
you solicit Code Review from at least two independent reviewers, and with an emphasis
on receiving review of both (a) the correctness of the SQL at achieving its intended
purpose and (b) the safety of achieving the intended purpose from the perspective of
business domain sensibility.

Pre-execution + QA
-------------------

Before executing the script, generate a backup of the database you will be running the script
on. This can be done with the Aptible CLI. E.g.

```
  aptible db:backup production-healthify-postgresql
```

You can view database backups in the Aptible Dashboard by viewing the show page of
a particular database and clicking the "Backups" tab. Take note of which backup
you just created (the backups are timestamped). In the event that there is a problem,
the backup can be used as the source of truth about what the affected database *should*
look like. Note: If there are mirrored backups, `us-east-1` and `us-west-1`, it is
better to restore using `us-east-1` because it will restore much more quickly.

Similiar to Code Review, QA should be performed by two independent developers, with
at least one of them not being a contributor to the script. Each QA-er should
independently perform the exact same workflow on staging as you intend for production:

1) Create a backup of the staging database.
2) Execute the script, confirming that it first performs the steps to ensure
  reversibility (e.g. creates temp tables of the necessary data).
3) Sign in to the app and explore its functionality, validating that no data was botched.

Having done this QA, now confirm that you can successfully reverse the script if
necessary. Execute your remediation strategy as planned for during development, and explore
the app to confirm that you were able to successfully undo the changes.

Once QA is complete, feel free to re-run the script if that would result in a desirable state for
staging.

Deployment
----------

Having successfully QA-ed the script on Staging, we can move to execute it on Production.

Prior to execution, make sure to alert the dev team and relevant members of the Leadership,
Customer Success, and/or Resource Network teams of the impending changes. Give them time
to provide any warnings or other feedback before you execute the script.

Assuming approval from them, perform the same steps from QA:
1) Create a backup of the production database.
2) Execute the script, confirming that it first performs the steps to ensure
  reversibility (e.g. creates temp tables of the necessary data).
3) Sign in to the app and explore its functionality, validating that no data was botched.


Post-Deploy Cleanup
-------------------

After a week has passed without noticeable incident from the script, it is time to
ensure cleanup of the databases. This entails (a) deprovisioning any unnecessary backup
databases that were created and (b) destroying any rescue tables that were created for
the script. Alert the dev team prior to performing this cleanup.
