# Slack

Description Here

## Setup Slack App

Step 1 : Visit https://api.slack.com, login and select "Your Apps".
![Your Apps](../../resources/channels/slack/slack-setup-001.png)

Step 2 : Select "Create an App".
![Create an App](../../resources/channels/slack/slack-setup-002.png)

Step 3 : Give the App a name and select which Slack workspace the app will appear in.
![App Name](../../resources/channels/slack/slack-setup-003.png)

Step 4 : Select "Incoming Webhooks" from the left-hand menu and then turn "Activate Incoming Webhooks" to "On".
![Activate Incoming Webhooks](../../resources/channels/slack/slack-setup-004.png)

Step 5 : Select "Add New Webhook to Workspace".
![Add New Webhook](../../resources/channels/slack/slack-setup-005.png)

Step 6 : Select the channel this webhook will post messages into.
![Select Channel](../../resources/channels/slack/slack-setup-006.png)

Step 7 : Copy the Webhook URL.  This will be the "target" element for a slack [channel](../../classes/database/channel-db.md)/[template](../../classes/database/template-db.md) in Syntinel.
![Copy Webhook Url](../../resources/channels/slack/slack-setup-007.png)

Step 8 : Select "Interactivity & Shortcuts" from left-hand menu.  Turn "Interactivity" to "On" and then copy the [slack cue reply url](#getting-the-slack-cue-reply-url) into the Request URL field.
![Setup Interactivity](../../resources/channels/slack/slack-setup-008.png)


## Getting the Slack Cue Reply Url

Instructions Here