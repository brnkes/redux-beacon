# Google Analytics

* [Setup](#setup)
* [Usage](#usage)
* [Event Definitions](#event-definitions)

----

### Setup

1. Sign up for Google Analytics and
   [create a new web property](https://support.google.com/analytics/answer/1008015?hl=en).

2. Add the
   [JavaScript Tracking Snippet](https://developers.google.com/analytics/devguides/collection/analyticsjs/)
   to your site. Update the tracking snippet with your [tracking Id](https://support.google.com/analytics/answer/1008080).

    > **[info] Tip**
    > during development and testing it is often helpful to use the debug
    > version of analytics.js. Follow the instructions
    > [here](https://developers.google.com/analytics/devguides/collection/analyticsjs/debugging)
    > to enable it.

#### _Multiple Trackers Setup_
 * This target supports named trackers. Add the following line to the tracking
   snippet for each named tracker:

   ```js
   ga('create', 'UA-XXXXX-Z', 'auto', 'clientTracker');
   ```
 * The tracker can be specified in each [event definition](#event-definitions)
   using the `tracker` property.

#### _Ecommerce Plugin Setup_
 * Add this line to the end of your tracking snippet: `ga('require',
   'ecommerce');`. This line must be added **after** you call `ga('create',
   'UA-XXXXX-Y')`.

    > **[warning]**
    > Google Analytics will _fail silently_ if you try to use these events
    > without adding the require call in your initial tracking code. It is also
    > **not** recommended to use GA's basic analytics plugin if you're also
    > going to use the enhanced ecommerce plugin.

#### _Enhanced Ecommerce Plugin Setup_
 * Add this line to the end of your tracking snippet: `ga('require',
   'ec');`. This line must be added **after** you call `ga('create',
   'UA-XXXXX-Y')`.

    > **[warning]**
    > Google Analytics will _fail silently_ if you try to use these events
    > without adding the require call in your initial tracking code. It is also
    > **not** recommended to use GA's basic analytics plugin if you're also
    > going to use the basic ecommerce plugin.

### Usage

```js
import { GoogleAnalytics } from 'redux-beacon/targets/google-analytics';

// Create or import an events map.
// See "getting started" pages for instructions.

const ga = GoogleAnalytics();

const gaMiddleware = createMiddleware(eventsMap, ga);
const gaMetaReducer = createMetaReducer(eventsMap, ga);
```

### Event Definitions

* [`pageView`](#pageview)
* [`event`](#event)
* [`timing`](#timing)
* [`socialInteraction`](#socialinteraction)
* [`exception`](#exception)
* [`ecommItem`](#ecommitem)
* [`ecommTransaction`](#ecommtransaction)
* [`ecommSend`](#ecommsend)
* [`ecommClear`](#ecommclear)
* [`ecommImpression`](#ecommimpression)
* [`ecommProduct`](#ecommproduct)
* [`ecommPromotion`](#ecommpromotion)
* [`ecommAction`](#ecomaction)

#### pageView
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/pages

```js
import { trackPageView } from 'redux-beacon/targets/google-analytics';

const pageView = trackPageView((action, prevState, nextState) => {
 return {
   page: /* fill me in */,
   title: /* (optional) */,
   location: /* (optional) */,
 };
}, /* (optional) tracker name */ );
```

> **[danger] Duplicate Page Views**
> the last line of the tracking snippet `ga('send', 'pageview')` hits Google
> Analytics with a page view that matches the first loaded route. If you're
> tracking page views using Redux Beacon, be sure to remove this line so the
> initial page load isn't recorded twice.

<br>

#### event
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/events

```js
import { trackEvent } from 'redux-beacon/targets/google-analytics';

const event = trackEvent((action, prevState, nextState) => {
  return {
    eventCategory: /* fill me in */,
    eventAction: /* fill me in */,
    eventLabel: /* (optional) */,
    eventValue: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### timing
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/user-timings

```js
import { trackTiming } from 'redux-beacon/targets/google-analytics';

const userTiming = trackTiming((action, prevState, nextState) => {
  return {
    category: /* fill me in */,
    var: /* fill me in */,
    value: /* fill me in */,
    label: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### socialInteraction
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/social-interactions

```js
import { trackSocialInteraction } from 'redux-beacon/targets/google-analytics';

const socialInteraction = trackSocialInteraction((action, prevState, nextState) => {
  return {
    network: /* fill me in */,
    action: /* fill me in */,
    target: /* fill me in */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### exception
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/exceptions

```js
import { trackException } from 'redux-beacon/targets/google-analytics';

const exception = trackException((action, prevState, nextState) => {
  return {
    description: /* (optional) */,
    isFatal: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

Don't need to track `description` or `isFatal`?

```js
import { trackException } from 'redux-beacon/targets/google-analytics';

const noop = () => {};
const exception = trackException(noop, /* (optional) tracker name */ );
```

<br>

#### ecommItem
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce
##### Requires:
[Ecommerce Plugin Setup](#ecommerce-plugin-setup)

```js
import { trackEcommItem } from 'redux-beacon/targets/google-analytics';

const ecommItem = trackEcommItem((action, prevState, nextState) => {
  return {
    id: /* fill me in */,
    name: /* fill me in */,
    sku: /* (optional) */,
    category: /* (optional) */,
    price: /* (optional) */,
    quantity: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### ecommTransaction
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce
##### Requires:
[Ecommerce Plugin Setup](#ecommerce-plugin-setup)

```js
import { trackEcommTransaction } from 'redux-beacon/targets/google-analytics';

const ecommTransaction = trackEcommTransaction((action, prevState, nextState) => {
  return {
    id: /* fill me in */,
    affiliation: /* (optional) */,
    revenue: /* (optional) */,
    shipping: /* (optional) */,
    tax: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### ecommSend
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce
##### Requires:
[Ecommerce Plugin Setup](#ecommerce-plugin-setup) or
[Enhanced Ecommerce Plugin Setup](#enhanced-ecommerce-plugin-setup)

```js
import { ecommSend } from 'redux-beacon/targets/google-analytics';

const ecommSendSignal = ecommSend(/* (optional) tracker name */);
```

<br>

#### ecommClear
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce
##### Requires:
[Ecommerce Plugin Setup](#ecommerce-plugin-setup) or
[Enhanced Ecommerce Plugin Setup](#enhanced-ecommerce-plugin-setup)

```js
import { ecommClear } from 'redux-beacon/targets/google-analytics';

const ecommClearSignal = ecommClear(/* (optional) tracker name */);
```

<br>

#### ecommImpression
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/enhanced-ecommerce
##### Requires:
[Enhanced Ecommerce Plugin Setup](#enhanced-ecommerce-plugin-setup)

```js
import { trackEcommImpression } from 'redux-beacon/targets/google-analytics';

const ecommImpression = trackEcommImpression((action, prevState, nextState) => {
  return {
    id: /* fill me in */,
    name: /* fill me in */,
    list: /* (optional) */,
    brand: /* (optional) */,
    category: /* (optional) */,
    variant: /* (optional) */,
    position: /* (optional) */,
    price: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### ecommProduct
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/enhanced-ecommerce
##### Requires:
[Enhanced Ecommerce Plugin Setup](#enhanced-ecommerce-plugin-setup)

```js
import { trackEcommProduct } from 'redux-beacon/targets/google-analytics';

const ecommProduct = trackEcommProduct((action, prevState, nextState) => {
  return {
    id: /* fill me in */,
    name: /* fill me in */,
    brand: /* (optional) */,
    category: /* (optional) */,
    variant: /* (optional) */,
    price: /* (optional) */,
    quantity: /* (optional) */,
    coupon: /* (optional) */,
    position: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### ecommPromotion
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/enhanced-ecommerce
##### Requires:
[Enhanced Ecommerce Plugin Setup](#enhanced-ecommerce-plugin-setup)

```js
import { trackEcommPromotion } from 'redux-beacon/targets/google-analytics';

const ecommPromotion = trackEcommPromotion((action, prevState, nextState) => {
  return {
    id: /* fill me in */,
    name: /* fill me in */,
    creative: /* (optional) */,
    position: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```

<br>

#### ecommAction
##### Docs:
https://developers.google.com/analytics/devguides/collection/analyticsjs/enhanced-ecommerce
##### Requires:
[Enhanced Ecommerce Plugin Setup](#enhanced-ecommerce-plugin-setup)

```js
import { trackEcommAction } from 'redux-beacon/targets/google-analytics';

const ecommAction = trackEcommAction((action, prevState, nextState) => {
  return {
    id: /* fill me in */,
    affiliation: /* (optional) */,
    revenue: /* (optional) */,
    tax: /* (optional) */,
    shipping: /* (optional) */,
    coupon: /* (optional) */,
    list: /* (optional) */,
    step: /* (optional) */,
    option: /* (optional) */,
  };
}, /* (optional) tracker name */ );
```
