---
description: Learn more about the guild service
---

# Guild Service

## Introduction

If you want to interact with your Discord Bot, it is important to retrieve your server from the guild cache. You also need to fetch the guild by your server id. This can be annoying if you do this over and over again. That's why we created the guild service for you. Currently, there's only one feature inside the service that grabs your guild. The service will be extended by more features and ideas in the future.

{% hint style="warning" %}
Wishes and ideas can be posted inside [\#ideas-and-features](https://discord.gg/xNFaVN4asB) channel in our Discord
{% endhint %}

## Basic Usage

{% hint style="danger" %}
Actually not implemented!
{% endhint %}

Working with the guild service is fairly simple. Create a new class and extend the guild service.

{% hint style="success" %}
You can use any method you find in the [discord.js](https://discord.js.org/#/docs/main/stable/general/welcome) documentation. The `this.guild` is only a wrapper for your own guild.
{% endhint %}

{% tabs %}
{% tab title="your-bot.component.ts" %}
```typescript
// If you extend the guild service, your class has access to
// this.guild => this is your own server
@Component()
export class YourBotComponent extends GuildService {

  /**
   * Fetch all members and the giving roles for it
   */
  public fetchAllMembers(): void {
    this.guild.members
      .fetch()
      .then((members: Collection<string, GuildMember>) => {
        members.forEach((member: GuildMember) => {
          // For testing, log some informations
          UtilsService.log(JSON.stringify({
            name: member.user.username,
            displayName: member.displayName,
            roles: member.roles.cache.mapValues((role: Role) => role.name)
          }, null, 2));
        });
      })
      .catch((err) => {
        // Handle your error
      });
  }
}
```
{% endtab %}
{% endtabs %}

