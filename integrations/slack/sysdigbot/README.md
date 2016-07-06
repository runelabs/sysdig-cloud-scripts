# sysdigbot
Use this python script to spin up a Sysdigbot, a chat bot that allows you to interact with Sysdig Cloud though Slack.

Currently Sysdigbot allows you to post custom events directly to Sysdig Cloud through chats in Slack. These chats can come from you and your teammates, or from any other app that you have integrated with Slack (think: code deploys, support tickets, marketing events, etc.) 

Check out [this blog post](insert link) for more info.

Note, this script utilizes the Sysdig Cloud python client (https://github.com/draios/python-sdc-client), a wrapper for the Sysdig Cloud REST API. 

# Install Instructions
1. `pip install -r requirements.txt` 
2. Go to this page to create a new Slack bot user: https://my.slack.com/services/new/bot
3. Once you are done with the bot creation wizard, the Slack API token will be available under _Integration Settings_. Make a copy of it
4. Browse to https://app.sysdigcloud.com/#/settings/user and copy the Sysdig Cloud API Token that you find under _Sysdig Cloud API_
5. `python bot.py --sysdig-api-token <sysdig_token> --slack-token <slack_token>`

Alternatively you can use our provided Dockerfile:

1. `docker build -t sdc-bot .`
2. `docker run -d --name sdc-bot -e SYSDIG_API_TOKEN=<sysdig_token> -e SLACK_TOKEN=<slack_token> sdc-bot`

# Usage

Sysdigbot will automatically translate each message it hears on Slack into a Sysdig Cloud event: 
`description [name=<eventname>] [severity=<1 to 7>] [some_tag_key=some_tag_value]*`

You can send messages directly to Sysdigbot, or invite Sysdigbot to any Slack channel to listen in. This channel listening behavior can be handy if there are other bots in the channel that post automatic notifications. 

## Available commands

* `!help` - shows this message
* `!post_event *description [name=<eventname>] [severity=<1 to 7>] [some_tag_key=some_tag_value]*` - note, the `!post_event` prefix is only necessary if you launch bot.py with the `--no-auto-events` parameter 

## Examples

* `!post_event load balancer going down for maintenance`

* `!post_event my test event name="test 1" severity=5`

* `!post_event name="test 2" severity=1 tag=value`

# Improvements

- Add more commands. For example, it would be very cool to have a `!get_data` or `!get_alert_notification` that mimic the behavior of the Python client API
- Better parsing from bot messages. For example, we can recognize when a github/jenkins bot posts a message, and automatically dissect the message into name/description/tags