---
date: 2019-01-02
---
<meta property="og:published_time" content="2019-01-02">

# The CSS Spectrum from Semantic Components to Pragmatic Utilities

A decade or two ago, we usually wrote HTML by hand, whether in templates or as entire pages. Either way, these templates contained raw HTML that formed a structure. They said **what** was on the page: the semantic meaning behind what presented to the user.

We used ordered or unordered lists, anchors for links, buttons for actions, headings of various priority, and two types of emphasis instead of the predetermined bold and italic elements.

We did this because it made a better experience for our users. They were more accessible, as these semantics could carry over to an audible presentation of the page, or to search engines, or to RSS or read-it-later readers.

But it also made it easier for authors. We had a common language that we could understand and build with. There were various patterns that could be universally understood. You could look at the source code and gleem the intent.

This extended to the styling layer of CSS. Cascading style sheets were separate documents to the pages themselves. Elements on pages would be classed, and in CSS visual rules would be defined for each class. Use the same class on multiple pages, and bam, they would look the same everywhere after having the same rules applied.

Class names could be anything authors desired. They could be single words. They could be multiple words separated by hyphens-like-a-kebab, or allJoinedTogetherWithCapitals. They could have their own little mini hierarchy and system. But, whatever you chose, it didn’t matter. As long as the class names in the HTML and in the CSS matched, they would be applied.

They mattered to authors though. They had to look at them as they made new pages, and evolved the CSS rules. The semantics mattered, and so names that reflected the meaning of the content more than it’s eventual look were desired. A redesign with a new color scheme or rearranged layout could mean throwing all the CSS away if you had names like ‘button-red’ or ‘left-sidebar’. Far better to call them ‘button-danger’ and ‘main-sidebar’, as between redesigns the meaning would stay much the same.

And so we made a rule that CSS class names should not reflect its appearance but its underlying meaning. We broke this rule when we just had to (`clearfix` anyone?), but doing so was generally regarded as not planning for the future, and, well, just cheating really.

In a time of components we can make some changes. We can get rid of many of these semantic CSS classes. Why? Because our components are our semantic layer now.

Remember the structuring of HTML was important for accessibility, but the naming of CSS was really just to make the authors lives easier. Whether a button that looked red to a user had the word ‘red’ in its name or a more considered, meaningful name made no difference to the user. It looked the same, acted the same, and tasted the same (back when UIs looked so good you could lick them).

Components let us organise the building blocks of our pages and UIs into reusables. We can define a DangerButton component, and encapsulated all the styling knowledge inside (whether CSS partials or CSS-in-JS). We can then use this DangerButton in all its glorious meaning again and again, on page after page. When we decide they should no longer be red but bright purple, we have one place to make changes, just like the semantic CSS days.

Our components names and props can convey the full set of meaning, and even better encapsulate not only the styling choices but the amount of HTML too. We don’t need to know the raw elements needed to make a functional link in a navigation bar, we can just use the NavLink component.

So is reusable CSS like we had before encapsulated components dead? I don’t think so.

Utility classes are ultra reusable thin slices of CSS that we assemble to form a complete picture. A DangerButton might have white text on a dark red background, and so we’d use the `text-white` and `bg-red-dark` utility classes. We might also sprinkle in `text-lg` and `padding-4` and `rounded`.

These class names are decidedly not semantic. And they seem to replicate the granularity of CSS rules themselves. Aren’t they just the same as that other villain, inline styles where our CSS is written inside our HTML?

No, because they provide a system. Dark red is defined to one particular shade of crimson and then shared everywhere. The large font size class, `text-lg`, also can have the exact unit size changed in one exact spot. They might not be semantic, but they don’t spill all their beans.

Plus, they are just plain handy. CSS is fast, caches well, easily made responsive, and works with React or Vue or Elm or Rails. You don’t have to decide which CSS-in-JS library is most in vogue. You don’t even (dare I say) need JS.

Components to utility classes. A spectrum from semantic meaning through to pragmatic efficacy. While it’s not perfect, it’s a nice mix of both worlds.
