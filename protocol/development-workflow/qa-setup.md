# QA Setup

To add each staging server's remote, perform the following in the terminal.

```bash
git remote add demo git@beta.aptible.com:healthify-staging/demo-healthify.git
git remote add hoth git@beta.aptible.com:healthify-staging/hoth-healthify.git
git remote add staging git@beta.aptible.com:healthify-staging/staging-healthify.git
git remote add tatooine git@beta.aptible.com:healthify-staging/tatooine-healthify.git
git remote add naboo git@beta.aptible.com:healthify-staging/naboo-healthify.git
```

> 📌 Note that you should add your public SSH key to aptible. To get that key, run `cat ~/.ssh/id_rsa.pub`, copy the output, and paste it in your Aptible SSH key settings.

Now that you have the remotes available, you should be able to push any branch to any staging server as needed. For example, `git push hoth head:master --force` would force push your current local branch to the `hoth` server and subsequently deploy that code.

But first a few things to note:

## Staging has a specific purpose

The `staging` server is reserved for third party QA. If you find yourself in need of that server, ping Shelby or someone dealing with integrations to confirm it is available.

## Check the server status

Before you deploy to any server, make sure it is not already taken by referencing the [server spreadsheet](https://docs.google.com/spreadsheets/d/1qZ5x80cYXHxACJbZ20W5MqH6ETRFDmaLTnsjaqen6O0/edit).
