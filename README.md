---
description: Learn more about our motivation for creating this Framework
---

# Introduction

## Summary

Creating a gamemode can become very frustrating and confusing. If procedural programming gets the upper hand, the result can be chaos.

For this reason we have made it our task to develop a framework with which it is easy to program object-oriented.

The advantages of object-oriented programming are obvious. Clean class abstraction, reusable code and easier overview of the individual methods and classes.

{% hint style="danger" %}
This framework is not a gamemode. It is meant to help you build your own fantastic gamemode.
{% endhint %}

## Features

Our goal is to provide you with a simple system that is easy to understand and intuitive to use.

* Completely Open Source
* Fully TypeScript
* No fixed folder structure
* Minimalistic setup
  * Unused systems are not loaded even if they are not used
* Fully integrated [DI-Container](basic-knowledge/di-container.md) on server/client side
* Intuitive helper methods to maintain the performance of your gamemode
  * Sophisticated [Decorators](basic-knowledge/decorators/)
  * [Helpful Service classes](shared/utilsservice.md)
* [DatabaseService](server/database.md) for easy use
* Discord
  * [OAuth Server Integration](server/discord/authentication.md)
  * [Bot Integration](server/discord/bot.md)
* [Docker Support](https://github.com/abstractFlo/atlas-starter-docker)
* ES6 Support Server/Client
* Rollup Module Bundler
  * Creates only one resource for your gamemode script
* [Own Lifecycle](basic-knowledge/lifecycle.md)
  * You can run different tasks before someone can play on your server

