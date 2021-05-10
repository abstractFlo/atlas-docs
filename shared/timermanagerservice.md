---
description: Learn more about the TimerManagerService
---

# TimerManagerService

## Introduction

Working with timers can be anoying if you forgot to clear the timers after job is done. To make your life easier, we have create a TimerManagerService. With this service, you can create timers every where in your gamemode and clear them out on other location. 

This is solved by a simple ES6 Map. You declare a name and your callback and now you are done.  
You can retrieve the timer back by its name and clear them out.

### The TimerManagerService

The TimerManagerService is a simple class with some methods.

```typescript
import { RunningTimerInterface } from '../interfaces/running-timer.interface';
import { UtilsService } from './utils.service';
import { container, singleton } from 'tsyringe';
import { AutoloadAfter } from '../decorators/loader.decorator';
import { getAtlasMetaData } from '../decorators/helpers';
import { TimerConstants } from '../constants/timer.constants';
import { TimerModel } from '../models/timer.model';
import { constructor } from '../types/constructor';

@AutoloadAfter({ methodName: 'load' })
@singleton()
export class TimerManagerService {

  /**
   * Contains all timers
   *
   * @type {Map<string, RunningTimerInterface>}
   * @protected
   */
  protected runningTimers: Map<string, RunningTimerInterface> = new Map<string, RunningTimerInterface>();

  /**
   * Register reflection data
   *
   * @param {Function} done
   */
  public load(done: CallableFunction): void {
    this.registerAndResolveTimers();
    UtilsService.logLoaded('TimerManagerService');
    done();
  }

  /**
   * Clear specific timer by name
   *
   * @param {string} timerName
   */
  public clearRunningTimer(timerName: string): void {
    const runningTimer = this.runningTimers.get(timerName);

    if (!runningTimer) return;

    this.clearTimer(runningTimer);
    this.runningTimers.delete(timerName);
  }

  /**
   * Clear all running timers and empty the map
   *
   */
  public clearAllRunningTimers(): void {
    const runningTimers = Array.from(this.runningTimers.values());

    runningTimers.forEach((runningTimer: RunningTimerInterface) => this.clearTimer(runningTimer));
    this.runningTimers.clear();
  }

  /**
   * Create new Interval and insert to map
   *
   * @param {string} timerName
   * @param {Function} callback
   * @param {number} interval
   * @return {TimerManagerService}
   */
  public createInterval(timerName: string, callback: CallableFunction, interval: number): TimerManagerService | void {
    if (this.runningTimers.get(timerName)) {
      UtilsService.logError(`There is a timer with name ${timerName} already registered`);
      return;
    }

    const identifier = UtilsService.setInterval(() => callback(this), interval);

    this.runningTimers.set(timerName, { type: 'interval', identifier });
    return this;
  }

  /**
   * Create new everyTick and insert to map
   *
   * @param {string} timerName
   * @param {Function} callback
   * @return {TimerManagerService}
   */
  public createEveryTick(timerName: string, callback: CallableFunction): TimerManagerService | void {
    if (this.runningTimers.get(timerName)) {
      UtilsService.logError(`There is a timer with name ${timerName} already registered`);
      return;
    }

    const identifier = UtilsService.everyTick(() => callback(this));

    this.runningTimers.set(timerName, { type: 'everyTick', identifier });
    return this;
  }

  /**
   * Create new every tick and insert to map
   *
   * @param {string} timerName
   * @param {Function} callback
   * @return {TimerManagerService}
   */
  public createNextTick(timerName: string, callback: CallableFunction): TimerManagerService | void {
    if (this.runningTimers.get(timerName)) {
      UtilsService.logError(`There is a timer with name ${timerName} already registered`);
      return;
    }

    const identifier = UtilsService.nextTick(() => callback(this));

    this.runningTimers.set(timerName, { type: 'nextTick', identifier });
    return this;
  }

  /**
   * Create new timeout and register inside map
   *
   * @param {string} timerName
   * @param {Function} callback
   * @param {number} duration
   * @return {TimerManagerService}
   */
  public createTimeout(timerName: string, callback: CallableFunction, duration: number): TimerManagerService | void {
    if (this.runningTimers.get(timerName)) {
      UtilsService.logError(`There is a timer with name ${timerName} already registered`);
      return;
    }

    const identifier = UtilsService.setTimeout(() => callback(this), duration);

    this.runningTimers.set(timerName, { type: 'timeout', identifier });
    return this;
  }

  /**
   * Clear timer based on type
   *
   * @param {RunningTimerInterface} runningTimer
   * @private
   */
  private clearTimer(runningTimer: RunningTimerInterface) {
    switch (runningTimer.type) {
      case 'nextTick':
        UtilsService.clearNextTick(runningTimer.identifier);
        break;
      case 'everyTick':
        UtilsService.clearEveryTick(runningTimer.identifier);
        break;
      case 'interval':
        UtilsService.clearInterval(runningTimer.identifier);
        break;
      case 'timeout':
        UtilsService.clearTimeout(runningTimer.identifier);
        break;
    }
  }

  /**
   * Create new timer based on type
   *
   * @param {TimerModel} timer
   * @param {Function} callback
   * @private
   */
  private startTimer(timer: TimerModel, callback: CallableFunction) {
    switch (timer.type) {
      case 'timeout':
        this.createTimeout(timer.identifier, callback, timer.duration);
        break;
      case 'interval':
        this.createInterval(timer.identifier, callback, timer.duration);
        break;
      case 'nextTick':
        this.createNextTick(timer.identifier, callback);
        break;
      case 'everyTick':
        this.createEveryTick(timer.identifier, callback);
        break;
    }
  }

  /**
   * Register and resolve all reflection timers
   *
   * @private
   */
  private registerAndResolveTimers(): void {
    let timerCount = 0;
    const timers = getAtlasMetaData<TimerModel[]>(TimerConstants.TIMERS, this, []);

    timers.forEach((timer: TimerModel) => {
      const instances = container.resolveAll<constructor<any>>(timer.targetName);

      instances.forEach((instance: constructor<any>) => {
        const instanceMethod = instance[timer.methodName];

        if (!instanceMethod) return;

        const method = instanceMethod.bind(instance);
        this.startTimer(timer, method);
        timerCount++;
      });
    });

    UtilsService.logRegisteredHandlers('TimerManagerService', timerCount);
  }
}

```

### Available Methods

{% hint style="warning" %}
Every callback has access to TimeManagerService with it's first argument
{% endhint %}

#### createInterval\(name: string, callback: CallableFunction, duration: number\)

```typescript
this.timerManagerService.createInterval(
  'nameYourInterval', 
  (timerManagerService: TimerManagerService) => {
    // Do what ever you want
  }, 
  1000
)
```

#### createTimeout\(name: string, callback: CallableFunction, duration: number\)

```typescript
this.timerManagerService.createTimeout(
  'nameYourTimeout', 
  (timerManagerService: TimerManagerService) => {
    // Do what ever you want
  }, 
  1000
)
```

#### createNextTick\(name: string, callback: CallableFunction\)

```typescript
this.timerManagerService.createNextTick(
  'nameYourNextTick', 
  (timerManagerService: TimerManagerService) => {
    // Do what ever you want
  }
)
```

#### createEveryTick\(name: string, callback: CallableFunction\)

```typescript
this.timerManagerService.createEveryTick(
  'nameYourEveryTick', 
  (timerManagerService: TimerManagerService) => {
    // Do what ever you want
  }
)
```

### Usage

The Usage is very simple, get the Service from DI-Container and create your timers.

{% tabs %}
{% tab title="your.component.ts" %}
```typescript
import { TimerManagerService } from '@abstractflo/atlas-shared';

@Component()
export class YourComponent {

    constructor(
      private readonly timerManagerService: TimerManagerService
  ) {}

  public foo(): void {
    this.timerManagerService
      .createInterval(
        'nameYourInterval', 
        (timerManagerService: TimerManagerService) => {
          // Do what ever you want
        }, 
      1000
    )
  }
}

```
{% endtab %}

{% tab title="other.component.ts" %}
```typescript
import { TimerManagerService } from '@abstractflo/atlas-shared';

@Component()
export class OtherComponent {

    constructor(
      private readonly timerManagerService: TimerManagerService
  ) {}

  public foo(): void {
    this.timerManagerService.clearTimer('nameYourInterval');
  }
}

```
{% endtab %}
{% endtabs %}

## Decorators

For easier handling, we've create a @Interval Decorator. You can decorate every method and this method would be executed for the interval time after the framework is booted.

#### @Interval\(name: string, duration: number\)

```typescript
import { Interval, TimerManagerService } from '@abstractflo/atlas-shared';

@Component()
export class YourComponent {

    constructor(
      private readonly timerManagerService: TimerManagerService
  ) {}
  
  @Interval('nameYourInterval', 1000)
  public foo(): void {
    // Do what ever you want
  }
}
```

