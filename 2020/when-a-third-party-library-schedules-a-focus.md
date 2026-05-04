---
date: 2020-05-28
---
<meta property="og:published_time" content="2020-05-28">

# When a third party library schedules a focus via `requestAnimationFrame()` and blows all your tests up

The Reach UI components are great. They offer accessible components that you’ll often need when building web apps — such as modal dialogs, menus, tabs and more. It’s also popular and therefore has been battled-tested to work with a range of clients.

However, needing to provide accessibility in any project, it sometimes has to make tradeoffs. One decision the [@reach/menu-button](https://reacttraining.com/reach-ui/menu-button/) package has is when to focus on the menu when opening. It’s not focused immediately — it’s asynchronously focused on the next frame. Here’s what they do:

```js
  // When the menu is open, focus is placed on the menu itself so that
  // keyboard navigation is still possible.
  useEffect(() => {
    if (state.isExpanded) {
      // @ts-ignore
      window.__REACH_DISABLE_TOOLTIPS = true;
      window.requestAnimationFrame(() => {
        focus(menuRef.current);
```

This leads to errors when running Jest:

```
console.error
Warning: An update to PopoverImpl inside a test was not wrapped in act(…).

When testing, code that causes React state updates should be wrapped into act(...):

act(() => {
  /* fire events that update state */
});
/* assert on the output */

This ensures that you're testing the behavior the user would see in the browser. Learn more at https://fb.me/react-wrap-tests-with-act
```

We can’t change the implementation, so our tests are forced to come up with a solution.

We need to wait to the next frame. We can do this by running `requestAnimationFrame()` ourself.

However, when testing React we need to ensure anything that could change state is wrapped in [`act()`](https://reactjs.org/docs/test-utils.html#act) so that it has rerendered. This function accepts a callback that returns a Promise (also known as an `async function`). So first we wrap `requestAnimationFrame()` in a Promise, then wrap that in `act()` — let’s call this `waitForNextFrame()` like so:

```js
export function waitForNextFrame(): Promise<undefined> {
  return act(() => {
    return new Promise((resolve) => {
      requestAnimationFrame(() => resolve());
    });
  });
}
```

Now we can run this after clicking on our menu:

```js
import { render, screen } from "@testing-library/react";
import user from "@testing-library/user-event";

beforeEach(async () => {
  render(<ComponentWithMenu />);
  user.click(screen.getByRole('button', { name: 'Open menu' });
  await waitForNextFrame();
});
```

We could also defensively run this after every test so any animation frame callbacks have been run:

```js
afterEach(waitForNextFrame);
```
