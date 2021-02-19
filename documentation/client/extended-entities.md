---
description: 'Learn more about our custom alt:V Internal Extends'
---

# Extends

## Introduction

We have create some extends for alt:V internals. This will help you do some basic stuff without the boilerplate. Extending means that we create new methods and properties on internal alt:V classes

{% hint style="success" %}
Extending classes in TypeScript, is done by overriding the prototype for a class like **Player** or **Vehicle**.
{% endhint %}

## Our Extends

### alt.WebView

We created a method to simple route inside your SPA

{% tabs %}
{% tab title="routeTo" %}
```typescript
export class YourComponent {
    
    /**
     * Get webviewService from DI-Container
     */
    constructor(
        private readonly webviewService: WebviewService
    ) {}
    
    /**
     * Route to specific url inside your SPA
     */
    public yourMethod(): void {
        this.webviewService.routeTo('/your/page');
    }
}
```
{% endtab %}
{% endtabs %}

