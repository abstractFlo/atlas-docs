---
description: Learn more about the Webview Service
---

# Webview Service

## Introduction

Working with webviews can be managed with this service. We guess, you will love it.

{% hint style="danger" %}
Keep in mind, our WebviewService is only working with **SPA \(Single Page Application\)**.
{% endhint %}

## Setup

The only thing you should do, is set up the webview registry. The first argument is the path to your webview, second argument is the event name to change the route.

{% tabs %}
{% tab title="bootstrap.ts" %}
```typescript
import { registerWebview } from '@abstractFlo/atlas-client';

// other stuff inside bootstrap.ts
registerWebview('http://your/path/to/your/webview', 'routeEventName');
```
{% endtab %}

{% tab title="your.component.ts" %}
```typescript
import { singleton } from 'tsyringe';
import { On } from '@abstractFlo/atlas-shared';
import { WebviewService } from '@abstractFlo/atlas-client';

@singleton()
export class PlayerAuthComponent {

  constructor(
      private readonly webviewService: WebviewService
  ) {}

  /**
   * Change route, set focus and show cursor
   */
  @On('yourEvent')
  public yourEvent(): void {
    this.webviewService
        .routeTo('/your/path')
        .focus()
        .showCursor();
  }
}
```
{% endtab %}
{% endtabs %}

### Available Methods

Here you can find all available methods if you get the webviewService instance.

#### getWebView\(\)

```typescript
// Get webView Instance
const webView = this.webviewService.getWebView();
```

#### routeTo\(route: string, ...args: any\[\]\)

```typescript
// Change route inside your SPA
this.webviewService.routeTo('/your/path', 'arg1', 'arg2');
```

#### showCursor\(\)

```typescript
// Show Cursor
this.webviewService.showCursor();
```

#### removeCursor\(\)

```typescript
// Remove last created cursor
this.webviewService.removeCursor();
```

#### removeAllCursors\(\)

```typescript
// Remove all cursors
this.webviewService.removeAllCursors();
```

#### emit\(eventName: string, ...args: any\[\]\)

```typescript
// Emit event to webView
this.webviewService.emit('eventName', 'arg1', 'arg2');
```

#### on\(eventName: string, listener: \(...args: any\[\]\) =&gt; void\)

{% hint style="warning" %}
This method is not needed, The **@OnGui\(\)** Decorator does the same, but for completeness it is listed here.
{% endhint %}

```typescript
// Listen to event from webView
this.webviewService.on('eventName', (...args: any[]) => {
    // Do whatever you want
});
```

#### destroy\(\)

```typescript
// Destroy the webview
this.webviewService.destroy();
```

#### focus\(\)

```typescript
// Add focus
this.webviewService.focus();
```

#### unfocus\(\)

```typescript
// Remove focus
this.webviewService.unfocus();
```

## Side notes

You can combine any method like a chain.

```typescript
this.webviewService
    .routeTo('/your/path')
    .showCursor()
    .focus();
```

