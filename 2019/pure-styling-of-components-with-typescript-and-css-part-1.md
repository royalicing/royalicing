---
date: 2019-03-16
---
<meta property="og:published_time" content="2019-03-16">

# Typed Subatomic Styling: Part 1 — Benefits of TypeScript and CSS

The benefits of components have been realized with systems such as React and VueJS. Styling these components has a number of approaches, from CSS classes written in SCSS, inline styles, and dynamically generated stylesheets via something like emotion.

Each styling approach has trade-offs in regards to reusability, performance, and developer experience. Also, special considerations often have to be made when doing server-side rendering.

In this post, we’ll explore TypeScript combined with functional CSS. I believe it offers big benefits over the other choices — it’s extremely reusable, fast, and offers a very pleasant developer experience.

---

## Why CSS?

CSS classes are fundamentally simple. They are a name given to a set of CSS rules. You can reuse classes on as many elements as you like. You can use them from JavaScript or you can use when generating HTML from the server. They are just straightforward.

Well, so they seem. With a semantically named class name, like `btn`, it is tempting and often deemed necessary to override rules. To add more specificity — “In this context, it should look like this. Otherwise, it should look like this… apart from on this particular page, and when it’s inside the footer.”

Does this look familiar?

```html
<button class="btn">Learn more</button>

<footer>
<button class="btn">Sign in</button>
</footer>
```

```css
.btn {
    padding: 0.5rem;
    color: white;
    background-color: green;
}

footer {
    .btn {
    color: blue;
    background-color: white;
    }    
}
```

Here we have a `.btn` rule that forms a base for all buttons — a green background with white text. But our footer requires a different color, so we override it when a `.btn` is within a `footer` on the page.

There’s nothing wrong with this! It’s what CSS is designed for, and it does well.

However, this approach can over time can degrade into lots of overriding, into unused code and fragile code that you are afraid to make changes to. All this makes the code harder to reason about because there’s two pieces of code interacting with each other: the CSS, and the component’s template.

You might not know what something will look like until you see it in the browser. You see it there, and if it’s wrong then you hunt down whatever CSS rules are producing that effect until it’s fixed.

(It’s similar to the concept of impure imperative code that produces side effects that end up affecting other code unrelated to it. A pure function has no side effects; all it can affect is its output.)

So how could our button work with functional CSS classes?

```html
<button class="padding-2 bg-green text-white">Learn more</button>

<footer>
    <button class="padding-2 bg-white text-blue">Sign in</button>
</footer>
```

```css
.padding-2 { padding: 0.5rem; }

.bg-green { background: green; }
.bg-white { background: white; }

.text-blue { color: blue; }
.text-white { color: white; }
```

Well, doesn’t that look stupid! Why would we do that? We’re repeating all the styling choices every time we have a button. For every button we’ll have to say `padding-2` and then its chosen background and text color. ”That’s not semantic,” you might say.

The thing is, I’m not writing every `<button>` raw like this. I’m using components, say in React. Define one `<Button>` component and use that everywhere.

We could write the markup with the two buttons like so using JSX:

```jsx
<Button variant="call-to-action">Learn more</Button>

<footer>
    <Button variant="link">Sign in</Button>
</footer>
```

And let’s define this `<Button>` component, using TypeScript to specify its props.

```jsx
import React from "react";

type ButtonVariant = "call-to-action" | "link";

interface ButtonProps {
    variant: ButtonVariant;
    children: string;
}
export function Button({ variant, children }: ButtonProps) {
    return (
    <button className={`padding-2 ${variant === "call-to-action" ? "bg-green text-white" : "bg-white text-blue"}`}>{children}</button>
    )
}
```

Here we say there are two variations of buttons: a call to action, and one that looks like a link. This is our semantic layer. Semantics are important in the HTML markup: what tag and attributes you use will affect audible screen readers.

Our class names do not affect this semantic layer that the user sees. The class names are us authors, and the desire to have a semantic layer there is for the benefit of letting us make sense of what we are authoring. Which is especially important as the code evolves over time.

Semantic class names look a bit like this:

1.  semantic `.btn-primary` class name
2.  transformed into semantic `$primary-color` SCSS variable
3.  transformed into presentational `background` CSS rule with `#0f6138` as a value

The semantic layer for this path of setting the background color for a primary button end up losing their semantic meaning when they are actually processed by the browser. The background property is set to a RGB color written in hexadecimal. The browser, and particularly the user, gains no additional information by the fact we used the word “primary” in the class name.

With our functional class names, the process looks like this:

1.  semantic component `<Button>`
2.  transformed into semantic prop `variant` equal to `"call-to-action"`
3.  transformed into presentational class name `.bg-green`
4.  transformed into presentational background CSS rule with `#0f6138` as a value

The result is the same, authoring retains the semantic layer. Our component translates our semantic intent into our presentational intent. It’s a clear function of `Semantics Props -> Semantic HTML and Presentational Styling`

Ok, have we gone too far with the names `bg-green`? Probably. There’s nothing wrong with semantic class names. In fact, they help us keep a design system consistent and understandable. My problem is that when we become so intent on making our CSS class names have meaning — meaning that doesn’t affect our users — we actually make our jobs harder then they need to be.

Why is it hard? It’s a lot easier to add to CSS than it is to change or remove it. An override will often just get the job done. Until you have to override that override. The extra specificity of selector often falls out of sync of the semantic meaning in several months time as the design is extended and revised. It could all actually be a much simpler system of class names if you could just rewrite it from scratch. So, in that way, the original semantic naming choices have lost a lot of their value.

If we write semantic HTML using semantic components, we have fulfilled our semantic needs for both the user and authors. Our class names fall wherever on the spectrum between semantic and presentational we feel works for us.

_(If the meaning of a color is the brand color, then call it that. If it just happens to be orange because the designer thought that looked good, why do we feel we have to invent a semantic name that the designer doesn’t share or apply consistently? Ideally you steer the designer so you have a better defined common language. But this often isn’t adopted by them.)_

Let’s rename some of our color classes to be more semantic.

```css
.padding-2 { padding: 0.5rem; }

.bg-primary { background: green; }
.bg-light { background: white; }

.text-link-color { color: blue; }
.text-light { color: white; }
```

```jsx
import React from "react";

type ButtonVariant = "call-to-action" | "link";

interface ButtonProps {
    variant: ButtonVariant;
    children: string;
}
export function Button({ variant, children }: ButtonProps) {
    return (
    <button className={`padding-2 ${variant === "call-to-action" ? "bg-primary text-light" : "bg-light text-link-color"}`}>{children}</button>
    )
}
```

These classes are extremely reusable. A `<Pill>` component might reuse the same classes. A `<PrimaryHeader>` might be colored the primary color and so could use the `.bg-primary` class. Often many elements will share the same spacing of paddings and margins.

We can build a reusable palette of classes. Subatomic elements that are combined with semantic HTML to make components. The CSS for these functional classes will very likely be smaller than many combined component-specific CSS files.

---

## Great Developer Experience with TypeScript

TypeScript can improve the developer experience with these classes. We can add simple functions that build these class name, checking for typos along the way.

First, let’s expand our styles a little:

```css
.padding-1 { padding: 0.25rem; }
.padding-2 { padding: 0.5rem; }
.padding-4 { padding: 1rem; }

.bg-primary { background: green; }
.bg-warning { background: orange; }
.bg-danger { background: red; }
.bg-light { background: #fafafa; }

.text-dark { color: #111; }
.text-link-color { color: blue; }
.text-danger { color: red; }
.text-light { color: white; }
```

Let’s make a function for the padding set of classes. You’ll notice they share a common prefix `padding-`. Available suffixes are `1`, `2`, and `4` — but not `3`, `6`, or `8`.

How can we make sure no-one would accidentally specify `padding-3`? We can use a TypeScript union type:

```ts
type PaddingSuffix = "1" | "2" | "4";
function padding(paddingSuffix: PaddingSuffix): string {
    return `padding-${paddingSuffix}`;
}
```

We would then use it like so:

```ts
const class1 = padding("1"); // "padding-1"
const class4 = padding("4"); // "padding-4"
const class3 = padding("3"); // TypeScript Error!
```

Passing `"3"` to our function would produce an error from TypeScript, which prevents our code from even compiling. We won’t find out that the `padding-3` class doesn’t exist when we go to the browser, we’ll find out right away as we are authoring our components.

In a tool like [VS Code](https://code.visualstudio.com/), we’ll even get autocompletion of the available suffixes!

Let’s apply the same technique for our background and text colors.

```ts
type BackgroundSuffix = "primary" | "warning" | "danger" | "light";
function background(backgroundSuffix: BackgroundSuffix): string {
    return `background-${backgroundSuffix}`;
}

type TextSuffix = "dark" | "link-color" | "danger" | "light";
function text(textSuffix: TextSuffix): string {
    return `text-${textSuffix}`;
}

background("primary");   // "background-primary"
background("danger");    // "background-danger"
background("dark-blue"); // TypeScript Error!

text("dark");    // "text-dark"
text("light");   // "text-light"
text("success"); // TypeScript Error!
```

It’s a very simple technique. The functional class names share a common prefix, which is the same name given to the TypeScript function that will produce them.

---

What other design elements can we look at? Our simple design system might have two font families: a body font and a monospace font for code. It might have a range of font sizes from XS to XXL. And it might have two font weights, regular (also known as 400) and bold (aka 700).

```css
.font-body {
    font-family: system-ui, BlinkMacSystemFont, -apple-system, Segoe UI, Roboto, Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}
.font-mono {
    font-family: Menlo, Monaco, Consolas, Liberation Mono, Courier New, monospace;
}

.font-xs { font-size: 0.75rem; }
.font-s { font-size: 0.875rem; }
.font-base { font-size: 1rem; }
.font-l { font-size: 1.5rem; }
.font-xl { font-size: 2rem; }
.font-xxl { font-size: 4rem; }

.font-400 { font-weight: 400; }
.font-700 { font-weight: 700; }

type FontFamily = "body" | "mono";
type FontSize = "xs" | "s" | "base" | "l" | "xl" | "xxl";
type FontWeight = "400" | "700";
type FontSuffix = FontFamily | FontSize | FontWeight;
function font(...fontSuffixes: Array<FontSuffix>): string {
    return fontSuffixes.map(suffix => `font-${suffix}`).join(" ");
}

/* Valid class names */
font("body");                // "font-body"
font("mono", "s");           // "font-mono font-s"
font("body", "base", "700"); // "font-body font-base font-700"
font("body", "700");         // "font-body font-700"
font("body", "xxl", "700");  // "font-body font-xxl font-700"

/* Invalid class names */
font("sans-serif");          // TypeScript Error!
font("body", "base", "500"); // TypeScript Error!
font("body", "xxxxxxl");     // TypeScript Error!
```

Notice on the first line how we can specify body without saying what font size and weight? This allows you to still take advantage of CSS’s natural approach of inheriting certain properties such as font size and weight from its ancestors.

---

## Summing up

TypeScript not only gives us safety from typos but ensures a restricted palette of fonts, spacing, and colors is stuck to. This palette is extremely reusable, acting as the subatomic elements of all our components.

Our component become simple mapping from semantic props to semantic HTML and class names — a single file to make changes to instead of separate template and stylesheets. We can predictably make changes instead finding a surprise in the browser, requiring us to hunt down and understand the complex nest of CSS selectors.

It’s a fast user experience. CSS is a known quantity, browsers have optimized it for decades. There’s no dynamically inserted stylesheets. You can serve a single CSS file that is easily cached.

Best of all, it works just as well on the server as in the browser. There’s no tricks required of collected all the styles used, or prerendering. We don’t need to integrate with babel or any other tooling. Its performance is predictable.

Future posts will look at working with media queries and breakpoints, at the `:hover`, `:focus`, and `:focus-within` pseudo classes, at using CSS variables, at more complex conditional logic in components, and integrating with [Tailwind](https://tailwindcss.com/), a popular functional (aka utility-first) CSS library.
