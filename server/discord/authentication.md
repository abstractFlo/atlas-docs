---
description: Learn more about Discord Authentication
---

# Authentication

## Introduction

Using discord as authentication layer, is fairly simple. There're a few simple steps to integrate this nice feature into your gamemode.

{% hint style="warning" %}
Keep in mind, authentication with Discord, has nothing to do with a whitelist or something. It only checks if the user owns a Discord Account with valid credentials.
{% endhint %}

## Installation

#### Step 1

Go to [https://discord.com/developers/applications](https://discord.com/developers/applications) and create an account and application if you don't already have one.

#### Step 2

Visit the **General Information** Page and store the **Client ID** and **Client Secret** inside your `.env`

#### Step 3

Go to OAuth 2 and set up the redirect URL and press save.  
This URL must be set inside `.env` under `DISCORD_REDIRECT_URL` too.

![Add Redirect URL](../../.gitbook/assets/authorization_redirect.png)

{% hint style="success" %}
That's all for Setup. The Framework does the rest for you.
{% endhint %}

## Example Usage

This is a simple integration inside the connection process for your gamemode. You must be familiar with the setup. This example does not explain the very basic part for creating components. Only the needed components. Any kind of code can be modified to fit your own needs.

{% hint style="warning" %}
This section describes the usage of our products on each side. This means our framework for server/client and [our CEF implementation with svelte](https://github.com/abstractFlo/altv-svelte-tailwind-typescript). If you have a different setup on CEF side, you can port the Svelte part to your preferred solution.
{% endhint %}

{% hint style="danger" %}
Keep in mind, this is only an example. The player properties for **discordToken, pendingLogin** and **discord** does not exists inside framework or [starter-template](https://github.com/abstractFlo/atlas-starter). You must [extend ](../extends.md#alt-player)the player by yourself.
{% endhint %}

### Server

{% hint style="info" %}
This is the Server side integration for Discord authentication
{% endhint %}

{% tabs %}
{% tab title="auth.component.ts" %}
```typescript
import { FrameworkEvent, On, Component } from '@abstractFlo/atlas-shared';
import { Player } from 'alt-server';
import {
  DiscordApiService,
  DiscordUserModel,
  EncryptionService,
  OnClient
} from '@abstractFlo/atlas-server';

@Component()
export class AuthComponent {

  constructor(
      private readonly discordApiService: DiscordApiService
  ) {}


  /**
   * Prepare player data on connection complete
   *
   * @param {Player} player
   */
  @OnClient('playerConnectionComplete')
  public playerConnect(player: Player): void {
    player.pendingLogin = true;
    player.discordToken = EncryptionService.sha256(player.tokenData);
    player.dimension = player.id * 1000;

    const authUrl = this.discordApiService.getAuthUrl(player.discordToken);
    player.emit('playerBeginAuthentication', authUrl);
  }

  /**
   * Find player by their Discord token and emit event to Player if true
   *
   * @param {string} token
   * @param {DiscordUserModel} discordUser
   */
  @On(FrameworkEvent.Discord.AuthDone)
  public discordAuthDone(token: string, discordUser: DiscordUserModel): void {
    const player = Player.all.find((player: Player) =>
        player.discordToken === token
    );

    if (player) {
      delete player.discordToken;

      player.pendingLogin = false;
      player.discord = discordUser;

      player.emit(FrameworkEvent.Discord.AuthDone);
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Client

{% hint style="info" %}
Client side integration for Discord authentication
{% endhint %}

{% tabs %}
{% tab title="auth.component.ts" %}
```typescript
import { FrameworkEvent, On, Component } from '@abstractFlo/atlas-shared';
import { OnGui, OnServer, WebviewService } from '@abstractFlo/atlas-client';

@Component()
export class AuthComponent {

  /**
   * Contains the discordUrl
   *
   * @type {string}
   * @private
   */
  private discordUrl: string;

  constructor(
      private readonly webviewService: WebviewService
  ) {}

  /**
   * Store the given Discord auth URL
   *
   * @param {string} authUrl
   */
  @OnServer('playerBeginAuthentication')
  public beginAuthentication(authUrl: string): void {
    this.discordUrl = authUrl;

    this.webviewService
        .routeTo('/discord')
        .focus()
        .showCursor();
  }

  /**
   * Return the Discord URL to webview
   */
  @OnGui('discordOpenUrl')
  public getDiscordOpenUrl(): void {
    this.webviewService.emit('discordOpenUrl', this.discordUrl);
  }

  /**
   * This method is triggered if the auth is successfully done
   */
  @OnServer(FrameworkEvent.Discord.AuthDone)
  public authenticationDone(): void {
    // Player is successfully authenticated
    // you can now do what ever you want
  }

}
```
{% endtab %}
{% endtabs %}

### Gui/CEF

{% hint style="warning" %}
This implementation is only a very basic setup. No styles, no layouts.
{% endhint %}

{% tabs %}
{% tab title="Discord.svelte" %}
```markup
<script lang="ts">
  import {onMount, onDestroy} from 'svelte';
  import {AltVService} from '../../services';

  let isLoading: boolean = false;

  /**
   * Listen for discordOpenUrl event from client
   */
  onMount(() => {
    AltVService.on('discordOpenUrl', processWindowOpen);
  });

  /**
   * Remove Eventlistener if component is destroyed
   */
  onDestroy(() => {
    AltVService.off('discordOpenUrl', processWindowOpen);
  });

  /**
   * Open new browser window after click on the button
   */
  function processWindowOpen(oAuthUrl: string): void {
    isLoading = true;
    window.open(oAuthUrl);
  }

  /**
   * Send the request to client for get the auth URL
   */
  function handleAuth(): void {
    isLoading = !isLoading;
    AltVService.emit('discordOpenUrl');
  }
</script>

<div>
  <h1>Discord Authentication</h1>
  {#if !isLoading}
  <p>This allows you to play without any registration</p>
  {:else}
  <p>
    Now you can press Alt + Tab to start the authentication process. <br>
    Or simply click the RE - OPEN WINDOW to proceed.s
  </p>
  {/if}
  <button class="btn" on:click={handleAuth}>
    {#if !isLoading}
    Start Authentication
    {:else}
    Re - Open Window
    {/if}
  </button>
</div>
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Congrats you have now successfully setup Discord authentication.
{% endhint %}

