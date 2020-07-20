# Reddit Modqueue Discord Bot

This bot will retrieve all items in the modqueue for a given subreddit and post them to the Discord of your choice. Configuration is done in three easy steps:

1. Create a Discord webhook
2. Create a Reddit app
3. Create an app with [Heroku](http://www.heroku.com)

## Discord webhook

Server Settings > Webhooks > Create Webhook

Name it, select the channel you want the reports to reside, and save the webhook URL for later.

## Reddit app

Navigate to www.reddit.com/prefs/apps/ and click the button to create an app.

* Name it
* Select script (Script for personal use. Will only have access to the developers accounts)
* Give it a description
* The about URL is traditionally a link to the repository
* Set the **redirect uri** to https://not-an-aardvark.github.io/reddit-oauth-helper/

Store the `client_id` (the string below **personal use script**) and the secret into the `client_id` and `client_secret` for our setup with Heroku in a minute.

* Navigate to https://not-an-aardvark.github.io/reddit-oauth-helper/
* Paste your Client ID and Client Secret
* Check the **Permanent** box
* Check all options
* Click the **Generate tokens** button at the bottom
* Store the **refresh token**

## Heroku app

If you're not familiar with Heroku, I'll avoid command line instructions and have you use their website. In which case you'll need to fork this repository (**Fork** button at the top of this GitHub page). Otherwise, you can simply clone it and deploy this code to your Heroku app via the command line interface.

* Sign up for a free [Heroku account](https://signup.heroku.com/dc)
* From your Dashboard, [Create a new app](https://dashboard.heroku.com/new-app). Name it something unique!
* You'll be navigatged to the **Deploy** page. In the **Deployment method** section, select the GitHub option and type `reddit-modqueue-discord` into the *repo-name* input, click the Search button, and click the **Connect** button for the repository
* Select the **Overview** tab and in the **Installed add-ons** section select the **Configure Add-ons** link. In the **Add-ons** section search for **Heroku Postgres** and select the **Hobby Dev &mdash; Free** Plan and click **Provision**

Next, navigate to **Settings** and click the **Reveal Config Vars** button in the Config Vars section to add the following keys and their appropriate values from the previous setup steps with Discord and Reddit. You should have an existing variable named `DATABASE_URL` from provisioning the **Heroku Postgres** instance. In the input boxes below, enter the appropriate keys and values:

`CLIENT_ID` &mdash; from Reddit setup

`CLIENT_SECRET` &mdash; from Reddit setup

`REFRESH_TOKEN` &mdash; from Reddit setup

`SUBREDDIT` &mdash; your subreddit name, e.g. for www.reddit.com/r/pics enter `pics`

`USER_AGENT` &mdash; create your own descriptive name. The `useragent` should be unique, descriptive, contain your username on Reddit, and the version number of your application. It's used by Reddit to identify your program. Here is reddit's documentation on it: www.github.com/reddit-archive/reddit/wiki/API#rules

`WEBHOOK` &mdash; from Discord setup

`SKIP_DISCORD` &mdash; optional flag, useful a first-time run to populate the database and not spam Discord

### Launch your worker

Finally, we're ready to launch the worker! Navigate to the **Overview** tab on your Heroku dashboard and under **Dyno formation** click the **Configure Dynos** link. On this page, click the **Edit** button (pencil) and toggle it to on.

## Free Heroku Hours

As long as you add a credit card to your account (which won't be charged), you'll be able to run your hobby dyno for free indefinitely. According to the [Free dyno hour pool](https://devcenter.heroku.com/articles/free-dyno-hours#free-dyno-hour-pool):

> Personal accounts are given a base of 550 free dyno hours each month. In addition to these base hours, accounts which verify with a credit card will receive an additional 450 hours added to the monthly free dyno quota. **This means you can receive a total of 1000 free dyno hours per month, if you verify your account with a credit card.**
