---
date: 2023-11-09
---
<meta property="og:published_time" content="2023-11-09">

# Think of your visual UI as progressive enhancement on an accessible core

So often making for the web as a sighted user I think of the visual experience first.

What’s the layout? Which color palette? How does it look on mobile?

I’ll then mock something up in Figma or throw together a demo in Tailwind. “Ah yes, I can see it now. Maybe I’ll tweak this color and that border.”

But what about accessibility? I want it to work for someone who will not see but hear the page with their screen reader.

“Ok, I better change my newsletter form to not be so minimal in its field labels. And I had two separate nav layouts, one for mobile and one for everything else — I’ll make sure screen reader users only hear one. Oops, this hover interaction only works with taps and clicks, I better add keyboard support.”

It leads to redesign and compromise and a guilty thought that this would all be easier if I didn’t have to worry about accessibility.

Progressive enhancement has changed the way I make web apps. Instead of JavaScript powering everything like in my jQuery days, I start off with what HTML is being produced by the server and which powerful CSS features I can leverage with today’s versatile web standards.

I propose adopting this mindset for accessibility too. Make a bulleted outline of the accessibility tree you expect to be on each page. Have that mockup then be the basis for your visual Figma mockup.

e.g. In the language of ARIA landmarks and roles, an accessibility tree mockup for a blog could be:

- Header (`banner` landmark)
- Primary nav (`nav` landmark named “primary”)
    - Nav items (`link` elements, one with `aria-current="page"`)
    - Search form (`search` landmark)
- Main content (`main` landmark)
- Newsletter form (`form` landmark named “newsletter”)
    - Email field (`textbox` role named “email”)
    - Subscribe button (`button` role named “subscribe”)
- Footer (`contentinfo` landmark)
    - Secondary nav (`nav` landmark named “footer links”)

Work out the patterns and possible compromises ahead of time in making your UI work for both keyboards and touch screens. Make the heard experience with a screen reader as great as the visual experience on a MacBook.

Take the accessibility-tree outline you have and use it to inform your end-to-end Playwright tests.

If a feature can’t easily be made accessible, rethink the feature. Do this earlier by thinking about the accessibility first.

Designing web apps mobile-first made building for small touch screens easier because you started with the most constrained device first. Take another step back and you get a similar benefit. Designing and building web apps accessibility-first is easier because you start with the most constrained experience first.

Instead of bolting on the accessible experience next sprint (or next quarter), use it as your foundation and then everything — from visual flare to rich interactions to dark mode — can be a flavor enhancer to make the visual experience sing.

Think of your visual UI as being progressive enhancement on an accessible core.
