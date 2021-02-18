---
description: Learn more about the shipped Constants
---

# Constants

## Introduction

Our Framework has some predefined constants they must be used if you want to use some internal services.

## Available Constants

This constants used to define our services and events for outside use. To prevent some upgrade errors, please use the constants instead of the name

```typescript
export const FrameworkEvent = {
  EventService: {
    EmitGuiEvent: 'event:emit:gui:event',
    ServerEmitGui: 'server:emit:gui',
    GuiOn: 'gui:on',
    GuiEmitServer: 'gui:emit:server'
  },
  Discord: {
    AuthDone: 'express:discordUser:accessDone'
  },
  Player: {
    SetRealTime: 'player:set:realtime'
  }
}

```

### Example

```typescript
import { singleton } from 'tsyringe';
import { FrameworkEvent} from '@abstractFlo/shared';
import { OnServer} from '@abstractFlo/client';


@singleton()
export class YourComponent{

  /**
   * This event is called, if the discord auth is done by server
   */
  @OnServer(FrameworkEvent.Discord.AuthDone)
  public authenticationDone(): void {
    // Do whatever you want
  }
}

```

