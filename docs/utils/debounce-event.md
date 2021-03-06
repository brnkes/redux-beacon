### `debounceEvent(msDelay, eventDef)`

If you have an event definition tied to a Redux action that fires too
frequently, you can use this util to limit the frequency of analytics events
(e.g. form input).

#### Parameters
 * `number` msDelay
   * the time in milliseconds that needs to elapse between event definition
   calls before the analytics event is dispatched to a target.
 * `function` [eventDef](../api/event-definition.md)

#### Example

```js
import { debounceEvent } from 'redux-beacon/utils';

// A normal event definition
const searchTerm = (action) => ({
  hitType: 'event',
  eventCategory: 'books',
  eventAction: 'search',
  eventLabel: action.payload
});

const eventsMap = {
  // Assume that SEARCH_TERM_ENTERED fires whenever a user types a character into
  // an input field.
  SEARCH_TERM_ENTERED: debounceEvent(1000, searchTerm)
  // The analytics event will only fire after 1s since the last entered character
};

// provide the events map when creating your middleware or meta reducer...
```
