---
description: 'Learn more about our custom alt:V Internal Extends'
---

# Extends

## Introduction

We have create some extends for alt:V Internals. This will help you do some basic stuff without the boilerplate. Extends means that we create new methods and properties on Internal alt:V Classes

{% hint style="success" %}
Extending classes in TypeScript, is done by override the prototype for a class like **Player** or **Vehicle**.
{% endhint %}

## Our Extends

### alt.WebView

We have create a method to simple route inside your SPA

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

