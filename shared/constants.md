---
description: Learn more about the shipped Constants
---

# Constants

## Introduction

Our framework has some predefined constants. They have to be used if you want to use some internal services.

## Available Constants

These constants are used to define our services and events for external usage. Please make sure that you use the constants instead of their values. Using the constants will prevent issues when you update the framework in the future.

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

