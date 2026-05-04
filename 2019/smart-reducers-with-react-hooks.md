---
date: 2019-01-13
---
<meta property="og:published_time" content="2019-01-13">

# Smart Reducers with React & Hooks

Hooks are on their way soon to React, which offer ways to manage component state and lifecycles in function components.

Also coming is the reducer, a pattern from the popular library Redux. While I think Redux can make apps more complicated than they need to be, I think reducers as a primitive offer some interesting approaches, and being built into React, perhaps new patterns too.

## A counter

So let’s take a simple example of a counter. A counter is backed by an integer value, which gets stored in the component’s state. This state is managed by a reducer: a pure function taking the previous state and an action of some sort, and based on the action will return a new state.

Convention has actions holding a `type` property, which for our counter will be one of: **“increment”, “decrement”, “reset”**. We handle each action type in the `switch` statement of `countReducer()`.

Three buttons are rendered: `+1`, `-1`, and `reset`. Each button has an `onClick` handler, which will call `dispatch()` with the particular action type: `increment`, `decrement`, or `reset`.

```jsx
import React, { useReducer, useCallback } from "react";
import ReactDOM from "react-dom";

const initialState = { count: 0 };

function countReducer(state, action) {
  switch (action.type) {
    case "reset":
      return initialState;
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function App() {
  const [state, dispatch] = useReducer(countReducer, initialState);

  return (
    <div className="App">
      <h1>{state.count}</h1>
      <button onClick={() => dispatch("decrement")}>-1</button>
      <button onClick={() => dispatch("increment")}>+1</button>
      <button onClick={() => dispatch("reset")}>Reset</button>
    </div>
  );
}
```

By themselves creating these three `onClick` handler functions has little overhead. However, because every time our component renders we are creating them from scratch and passing new functions to each button’s `onClick`, React has to remove the old handlers and add the new ones.

It would be better if we didn’t create these handler functions from scratch each time our component renders. If we create them once, and then reuse them, then React wouldn’t have to perform the work of changing the handlers for the buttons.

Can we avoid the creation of three `onClick` handlers? Yes, we can with custom data attributes. These attributes must be prefixed with `data-` but everything that follows can be named as we like. We can access these attributes in JavaScript via the DOM’s `dataset` property.

So, for each button we could set a `data-action-type` data attribute, and then read that in the click handler. We could also use the `useCallback` hook to memoize this click handler.

```jsx
import React, { useReducer, useCallback } from "react";
import ReactDOM from "react-dom";

const initialState = { count: 0 };

function countReducer(state, action) {
  switch (action.type) {
    case "reset":
      return initialState;
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function App() {
  const [state, dispatch] = useReducer(countReducer, initialState);
  const onClick = useCallback(
    (event) => {
      // Find our <button>’s DOM element.
      const buttonEl = event.target;
      // Read the button’s data-action-type attribute.
      // Note the kebab-case becomes camelCase.
      const type = buttonEl.dataset.actionType;
      // Dispatch an action of this button’s type to the reducer.
      dispatch({ type });
    },
    [dispatch]
  );

  // Note each button gets its own `data-action-type`,
  // but shares the same `onClick` prop.
  return (
    <div className="App">
      <h1>{state.count}</h1>
      <button data-action-type="decrement" onClick={onClick}>
        -1
      </button>
      <button data-action-type="increment" onClick={onClick}>
        +1
      </button>
      <button data-action-type="reset" onClick={onClick}>
        Reset
      </button>
    </div>
  );
}
```

So this should avoid in re-renders the need to remove and add click handlers. Not only are we using a single `onClick` handler for each button, by using the `useCallback` hook we ensure the component will creating it only once on the initial render and then reuse it in future renders.

Out of interest, could we go further? Could we reduce what we need to pass to each button and make it nicer to removing the `data-` from view?

```jsx
import React, { useReducer, useCallback } from "react";
import ReactDOM from "react-dom";

const initialState = { count: 0 };

function countReducer(state, action) {
  switch (action.type) {
    case "reset":
      return initialState;
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function App() {
  const [state, dispatch] = useReducer(countReducer, initialState);
  const onClick = useCallback(
    (event) => {
      // Find our <button>’s DOM element.
      const buttonEl = event.target;
      // Read the button’s data-action-type attribute.
      // Note the kebab-case becomes camelCase.
      const type = buttonEl.dataset.actionType;
      // Dispatch an action of this button’s type to the reducer.
      dispatch({ type });
    },
    [dispatch]
  );
  // Define a memoized component that dispatches actions to our reducer
  const ActionButton = useCallback(
    ({ actionType, ...rest }) => {
      // Map from the nice `actionType` prop into the
      // data attribute `data-action-type`.
      // Also pass the `onClick` which every button will share.
      return (
        <button {...rest} data-action-type={actionType} onClick={onClick} />
      );
    },
    [onClick]
  );

  return (
    <div className="App">
      <h1>{state.count}</h1>
      <ActionButton actionType="decrement">-1</ActionButton>
      <ActionButton actionType="increment">+1</ActionButton>
      <ActionButton actionType="reset">Reset</ActionButton>
    </div>
  );
}
```

Now, you might be thinking: this is just a simple counter demo. What about passing a payload when we dispatch our actions?

One approach could be to add another prop to our `ActionButton` component, something like `actionPayload`. However, if we were to also pass this as a data attribute, we would have to serialise the value, which would add extra overhead and make our component slower.

So, instead let’s rely on the fact that our component are meant to be pure: given a set of props taken as input, then the same output should be rendered every time.

Another way to say this is everything in our component, from the elements rendered to the code inside the event handlers, is derived from our props.

This mean the desired custom action is also derived from our props: given a set of props, the same payload will be calculated every time. Therefore, we can calculate the payload needed on-demand when we dispatch.

Here I have added two new action creators: “increment.by10” and “decrement.by10”. I am using a dot to denote these custom actions, and to show which actual action type will be dispatched.

```jsx
import React, { useReducer, useCallback } from "react";
import ReactDOM from "react-dom";

const initialState = { count: 0 };

function countReducer(state, action) {
  const payload = action.payload || {};
  switch (action.type) {
    case "reset":
      return initialState;
    case "increment":
      return { count: state.count + (payload.amount || 1) };
    case "decrement":
      return { count: state.count - (payload.amount || 1) };
    default:
      return state;
  }
}

function App() {
  const [state, dispatch] = useReducer(countReducer, initialState);
  const onClick = useCallback(
    (event) => {
      const buttonEl = event.target;
      const type = buttonEl.dataset.actionType;
      switch (type) {
        // Our custom action creators
        case "increment.by10":
          dispatch({ type: "increment", payload: { amount: 10 } });
          break;
        case "decrement.by10":
          dispatch({ type: "decrement", payload: { amount: 10 } });
          break;
        // Normal reducer action
        default:
          dispatch({ type });
      }
    },
    [dispatch]
  );
  const ActionButton = useCallback(
    ({ actionType, ...rest }) => {
      return (
        <button {...rest} data-action-type={actionType} onClick={onClick} />
      );
    },
    [onClick]
  );

  return (
    <div className="App">
      <h1>{state.count}</h1>
      <ActionButton actionType="decrement">-1</ActionButton>
      <ActionButton actionType="increment">+1</ActionButton>
      <ActionButton actionType="decrement.by10">-10</ActionButton>
      <ActionButton actionType="increment.by10">+10</ActionButton>
      <ActionButton actionType="reset">Reset</ActionButton>
    </div>
  );
}
```

The reducer has been changed to accept payloads for the “increment” and “decrement” action. However it still remains a pure function, like reducers should be. It’s also lean: we could add “increment.by50” or “increment.by1000” without needing to change our reducer.

You can see or fork an online demo of the above counter here: [https://codesandbox.io/s/jlz8pk6yow](https://codesandbox.io/s/jlz8pk6yow)

Thanks for reading, and if you have any thoughts [reach out on Twitter](https://twitter.com/royalicing).
