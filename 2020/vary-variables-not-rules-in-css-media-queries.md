---
date: 2020-11-24
---
<meta property="og:published_time" content="2020-11-24">

# Vary variables not rules in CSS media queries

A common pattern you’ll see when implementing responsive design with CSS is to redefine rules in media queries:

```css
h1 {
  font-size: 2rem;
}

@media (min-width: 720px) {
  h1 {
    font-size: 2.5rem;
  }
}

@media (min-width: 1200px) {
  h1 {
    font-size: 3rem;
  }
}
```

Here we are writing our media queries _mobile first_. This means the styles for small devices (like phones) are defined first, such as our font size of `2rem` for `h1` elements. Then a wider device is targeted with a media query that increases the font size of h1 elements to `2.5rem` when the viewport width is `720px` or greater. And then another one, increasing to `3rem` for viewports `1200px` or wider.

So what are the issues with this approach?

First, these media queries can get bloated. You’ll find that you’ll define your base rules first using a mobile-first approach, and then repeat those rules with overrides within media queries. You could put all of these rules for the same media query together underneath, or could you have the media query override live next to its mobile-first rule and rely on some sort of CSS optimiser or compression so the repetition doesn’t contribute to an increase in file size.

Second, knowing what to override can lead to complex code. Perhaps you only need to override the rules for `nav a` and not for `nav` elements within your media queries. These specifics lead to harder to understand code.

```css
nav {
  background-color: lightskyblue;
}
nav a {
  display: block;
  padding: 0.5rem;
}

@media (min-width: 720px) {
  /* We override the rule for `nav a` but not for `nav` */
  nav a {
    padding: 1rem;
  }
}
```

Perhaps you used quite complex selectors that now need to be repeated for each media query. SCSS allows the media query to be written nested inside the rule, avoiding the need to repeat it as an author. But this still adds a layer of complexity that can make understanding which rule applies in what case that much harder.

Here’s the above two examples but written to take advantage of SCSS’s nesting of media queries:

```scss
h1 {
  font-size: 2rem;

  @media (min-width: 720px) {
    font-size: 2.5rem;
  }
  @media (min-width: 1200px) {
    font-size: 3rem;
  }
}

nav {
  background-color: lightskyblue;

  a {
    display: block;
    padding: 0.5rem;

    @media (min-width: 720px) {
      padding: 1rem;
    }
  }
}
```

The result is very direct — these rules have these specific media query overrides. But there a lot of indentation going on, and things get hard to read once you add more rules and more overrides.

In summary, by redefining our rules inside media queries they have a lot of responsibility. They contain knowledge about our selectors. They override some rules but usually not all of them.

## Varying CSS custom properties

So what’s the alternative? If we start using custom properties (aka CSS variables), which we can do if we no longer support Internet Explorer, then our media queries can take on a lot less responsibility and thereby be a lot simpler.

```css
/* Define our variables, with mobile-first values */

:root {
  --heading-1-size: 2rem;
  --nav-link-padding: 0.5rem;
}

/* Override just the variables for different viewports */

@media (min-width: 720px) {
  :root {
    --heading-1-size: 2.5rem;
    --nav-link-padding: 1rem;
  }
}

@media (min-width: 1200px) {
  :root {
    --heading-1-size: 3rem;
  }
}

/* Define all our rules, consuming those variables */

h1 {
  font-size: var(--heading-1-size);
}

nav {
  background-color: lightskyblue;
}
nav a {
  display: block;
  padding: var(--nav-link-padding);
}
```

Making our variables responsive lets our rules be a lot simpler. Everything is flatter without the multiple levels of indentation. And we don’t need to use SCSS to attempt to reduce repetition, we can just stick with CSS!

This works not only for viewport width checks but also for media queries that check for dark mode or reduced motion or which features are supported.

```css
:root {
  --text-color: white;
}

@media (prefers-color-scheme: light) {
  /* We respect a user's browser preference */
  :root {
    --text-color: black;
  }
}

[data-mode="light"] {
  /* We allow users to switch light/dark mode */
  --text-color: black;
}

p,
h1,
h2,
h3,
h4,
figure,
ul,
ol {
  color: var(--text-color);
}
```

Our CSS rules can be none the wiser to what value a `--text-color` is under the hood, which is set to white by default and black when the user prefers a light color scheme. Our variables vary and our rules stay the same.
