---
date: 2019-05-22
---
<meta property="og:published_time" content="2019-05-22">

# Linked: The case of partial hydration (with Next and Preact)

Lukas Bombach has [written an article](https://medium.com/spring-media-techblog/how-we-achieved-the-best-web-performance-with-partial-hydration-20fab9c808d5) on partially hydrating server-rendered HTML so that less client code is needed. Hydrating becomes an opt-in process per top-level component. This is used to make a performant news site.

> Now you end up sending your entire application code to your users including React components for every headline or text paragraph anywhere on your page. The result is an unnecessarily huge bundle that needs to be loaded, parsed and executed. This results in suboptimal performance, your page will be slow(er) especially for mobile users and for no good reason!
> …
> The issue with Gatsby is that it has to generate all of your pages and subpages at compile time which does not really work when you have sites linked to a CMS that updates everyday and that hosts millions of articles—which is exactly what we need for our news media sites. This is why we are using Next as well as modifying it to suit our needs.
> …
> The basic idea behind our version of partial hydration is: Instead of doing SSR and then sending your entire application to your client, only parts of your application’s JavaScript would be sent to the client to hydrate the parts of your website which specifically require JavaScript to work.
