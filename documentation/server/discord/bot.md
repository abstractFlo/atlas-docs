---
description: Learn more about the Discord Bot
---

# Bot

## Introduction

Integrate a Discord bot to your gamemode can have some benefits for you. Creating a whitelist, use the awesome permission layer from Discord itself for in-game actions, read some server stats, kick players and so on. The list can go further.

{% hint style="info" %}
Creating your own bot cannot be simpler than this.
{% endhint %}

## Setup the Bot

### Step 1

Go to [https://discord.com/developers/applications](https://discord.com/developers/applications) and create an account and application if you do not already have on.

### Step 2

Visit the Page **Bot** and **Add Bot** or use an existing one.

### Step 3

Store the Bot Token inside your `environment.json` and set it up based on the picture below.

![Uncheck Public Bot and Check Privileged Gateway and Presence Intent](../../../.gitbook/assets/intents.png)

### Step 4

Generate an OAuth URL and setup permissions for your bot, copy and paste it into your browser and invite the bot to your server.

![We prefer to check only the permissions you really want](../../../.gitbook/assets/permissions_oauth.png)

![Invite the bot to your server](../../../.gitbook/assets/invite_the_bot.png)

{% hint style="success" %}
That's all for the setup. The framework does the rest for you.
{% endhint %}

## Example Usage

This example is not a real world example. It will only show you the usage with the decorator and the interaction with the bot.

{% hint style="warning" %}
To keep it simple as possible, there is a decorator for you to interact with the discord bot events.  
Any available bot event from [discord.js](https://discord.js.org/#/) are also useable with the decorator. The event params will be moved to the method.

Keep in mind, the decorator is only available server side
{% endhint %}

{% tabs %}
{% tab title="Listen for GuildMemberUpdate" %}
```typescript
export class MyComponent {

  /**
   * Do something if the guildMemberUpdate event fired
   *
   * @param {GuildMember} oldMember
   * @param {GuildMember} newMember
   */
  @OnDiscord('guildMemberUpdate')
  public doSomething(oldMember: GuildMember, newMember: GuildMember): void {
    // Now you can do something with the oldMember and the newMember
    // e.g. Check if a roles exists and kick if not
  }

}
```
{% endtab %}
{% endtabs %}

