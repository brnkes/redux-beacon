# Getting Started (Redux Users)
[Step 1](#step-1), [Step 2](#step-2), [Step 3](#step-3), [Step 4](#step-4), [Step 5](#step-5), [Next Steps](#next-steps)

### Step 1
Choose a target, and follow the setup instructions.

 * [GoogleAnalytics: Setup](targets/google-analytics.md#setup)
 * [GoogleAnalyticsGtag: Setup](targets/google-analytics-gtag.md#setup)
 * [GoogleTagManager: Setup](targets/google-tag-manager.md#setup)
 * [Segment: Setup](targets/segment.md#setup)
 * [Amplitude: Setup](targets/amplitude.md#setup)
 * React Native:
   * [GoogleAnalytics: Setup](targets/react-native-google-analytics.md#setup)
   * [GoogleTagManager: Setup](targets/react-native-google-tag-manager.md#setup)

<br>
> **[info]**
> Don't see the target you need?
> Follow the instructions [here](customize/create-own-target.md) to build your own.

### Step 2
Decide what you want to track and pick out the corresponding event definition:

 * [GoogleAnalytics: Event Definitions](targets/google-analytics.md#event-definitions)
 * [GoogleAnalyticsGtag: Event Definitions](targets/google-analytics-gtag.md#event-definitions)
 * [GoogleTagManager: Event Definitions](targets/google-tag-manager.md#event-definitions)
 * [Segment: Event Definitions](targets/segment.md#event-definitions)
 * [Amplitude: Event Definitions](targets/amplitude.md#event-definitions)
 * React Native:
   * [GoogleAnalytics: Event Definitions](targets/react-native-google-analytics.md#event-definitions)
   * [GoogleTagManager: Event Definitions](targets/react-native-google-tag-manager.md#event-definitions)

### Step 3
Match the event definition to a Redux action.

For example, say you want to intercept a `PLAY_VIDEO` Redux action and track it
as a Google Analytics event:

```js
// Import your analytics target
import { GoogleAnalytics } from 'redux-beacon/targets/google-analytics';
// Import your event definition helper (if applicable)
import { makeEvent } from 'redux-beacon/targets/google-analytics';

// Copy & paste the event definition you chose in step 2:
const event = makeEvent((action, prevState, nextState) => {
  return {
    eventCategory: /* fill me in */,
    eventAction: /* fill me in */,
    eventLabel: /* (optional) */,
    eventValue: /* (optional) */,
  };
}, /* (optional) tracker name */ );

// Match the event definition to a Redux action:
const eventsMap = {
  'PLAY_VIDEO': emitEvent,
};
```

### Step 4

Complete the event definition by filling in the object properties, then create the
middleware.

A continuation of the previous example:

```js
import { GoogleAnalytics } from 'redux-beacon/targets/google-analytics';
import { makeEvent } from 'redux-beacon/targets/google-analytics';
// Get Redux Beacon's middleware creator
import { createMiddleware } from 'redux-beacon';
// (optional)
import { logger } from 'redux-beacon/extensions/logger';


// Complete the event definition
const emitVideoPlayed = makeEvent((action) => ({
  eventCategory: 'Video',
  eventAction: action.type,
}));

const eventsMap = {
  'PLAY_VIDEO': emitVideoPlayed, // update if renamed
};

// Create the middleware
const ga = GoogleAnalytics();
const gaMiddleware = createMiddleware(eventsMap, ga, { logger });

```

Follow your target's instructions to create it:

 * [GoogleAnalytics: Usage](targets/google-analytics.md#usage)
 * [GoogleAnalyticsGtag: Usage](targets/google-analytics-gtag.md#usage)
 * [GoogleTagManager: Usage](targets/google-tag-manager.md#usage)
 * [Segment: Usage](targets/segment.md#usage)
 * [Amplitude: Usage](targets/amplitude.md#usage)
 * React Native:
   * [GoogleAnalytics: Usage](targets/react-native-google-analytics.md#usage)
   * [GoogleTagManager: Usage](targets/react-native-google-tag-manager.md#usage)

### Step 5
Follow the instructions [here](https://redux.js.org/docs/api/applyMiddleware.html) to
apply the middleware to your store.

----

### Next Steps

##### Track More Events:
Repeat steps [2](#step-2) and [3](#step-3) for every event you want to track. In a nutshell:
  1. Find the event definition for the thing you want to track.
  2. Match the event definition to a Redux action then complete the event definition.

##### Decide Where to put Analytics Logic:
As the number of events you track grows, it might make sense to move your event
definitions and events map into their own files. Redux Beacon has no
requirements on where these are kept. Have a discussion with your team on how
you'd like to organize your app's analytics logic.

##### Check Out the Recipes:
We have a number of recipes to get you started with Redux Beacon. Check them
out [here](recipes/index.md).

##### Check Out the Extensions:
We have extensions for logging and for buffering analytics events
offline. Check them out [here](extensions/index.md).
