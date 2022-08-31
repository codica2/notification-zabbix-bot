# Zabbix bot for notification in slack!

![](zabbix-bot.png)

---

### Configuration
-------------

This guide describes how to integrate your Zabbix 5.4 and higher installation with Slack using the Zabbix webhook feature. This guide will provide instructions on setting up a media type, a user and an action in Zabbix.

### Setting up a Slack bot

1. On the page [Your Apps](https://api.slack.com/apps) press **Create New App** and specify its name and workspace.

2. In the **Add features and functionality** section, select **Bots** and press **Review Scopes to Add**.

3. In the **Scopes** section, find **Scopes**, press **Add an OAuth Scope** and add `chat:write`, `im:write` and `groups:write` scopes.

4. Press **Install to Workspace** on the top of the page.

5. Now you have bot token, but you only need to use **Bot User OAuth Access Token**.

### Zabbix Webhook configuration

**Create a global macro**

1. Before setting up the **Webhook**, you need to setup the global macro `{$ZABBIX.URL}`, which must contain the **URL** to the **Zabbix frontend**.

2. In the **Administration** > **Media** types section, import the [slack-notification.js](slack-notification.js).

3. Open the added **Slack** media type and set **bot_token** to the previously created token.

- You can also choose between two notification modes:
  - alarm (default)
    - Update messages will be attached as replies to Slack message thread
    - Recovery message from Zabbix will update initial message
      ```**Note**: alarm mode works correctly only if you send messages to only one channel because the webhook uses non-unique event tags for storing data about created threads, and we cannot perform multiple threads updating.```
  - event
    - Recovery and update messages from Zabbix will be posted as new messages.

4. Click the **Update** button to save the **Webhook** settings.

5. To receive notifications in **Slack**, you need to create a **Zabbix user** and add **Media** with the **Slack** type.

Add new **Parameters:**

- `down_site_url` `{TRIGGER.URL}`
- `duration` `{EVENT.DURATION}`
- `event_ack_status` `{EVENT.ACK.STATUS}`
- `event_name` `{EVENT.NAME}`
- `event_update_message` `{EVENT.UPDATE.MESSAGE}`
- `host_ip` `{HOST.IP}`
- `host_web` `{TRIGGER.NAME}`

The **Send to** field can contain several variants of values:

- Channel name in `#channel_name` format
- User name in `@slack_user` format for direct messages
- Identifier (for example: `GQMNQ5G5R`)

6. You must add your bot to the target channel

7. Add **Macros** in Zabbix

- **Administration** > **General** > **Macros**
  - `{$SLACKTOKEN}` `Your Slack Tocken`
  - `{$SNMP_COMMUNITY}` `public`
  - `{$ZABBIX.URL}` `Your Zabbix URL`

For more information, use the [Zabbix](https://www.zabbix.com/documentation/6.0/manual/config/notifications) and [Slack API](https://api.slack.com/) documentations.

### Supported Versions
`Zabbix 5.4`

## License
Slack Notification is Copyright © 2015-2021 Codica. It is released under the [MIT License](https://opensource.org/licenses/MIT).

## About Codica

[![Codica logo](https://www.codica.com/assets/images/logo/logo.svg)](https://www.codica.com)

We love open source software! See [our other projects](https://github.com/codica2) or [hire us](https://www.codica.com/) to design, develop, and grow your product.
