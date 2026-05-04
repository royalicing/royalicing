---
title: Mocking window.location in Jest
date: 2021-05-21
---
<meta property="og:published_time" content="2021-05-21">

# Mocking `window.location` in Jest

It’s sometimes necessary to mock `window.location` in Jest tests. This could because you want to know when `window.location.reload()` is called, or one of its other methods (`.assign()`, `.replace()`).

Here’s some code to achieve this:

```js
const savedLocation = window.location;

beforeEach(() => {
  delete window.location;
  window.location = Object.assign(new URL("https://example.org"), {
    ancestorOrigins: "",
    assign: jest.fn(),
    reload: jest.fn(),
    replace: jest.fn()
  });
});
afterEach(() => {
  window.location = savedLocation;
});
You could then detect whether your implementation called .reload():

it("reloads the page", () => {
  expect(window.location.reload).toHaveBeenCalled();
});
```

If you wanted to reuse this in multiple tests, you could wrap this in a reusable function:

```js
function mockWindowLocation() {
  const savedLocation = window.location;

  beforeEach(() => {
    delete window.location;
    window.location = Object.assign(new URL("https://example.org"), {
      ancestorOrigins: "",
      assign: jest.fn(),
      reload: jest.fn(),
      replace: jest.fn()
    });
  });
  afterEach(() => {
    window.location = savedLocation;
  });
}
```
