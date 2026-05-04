---
date: 2019-05-25
---
<meta property="og:published_time" content="2019-05-25">

# Low Overhead React

_“React. Write reusable components. Make your data flow one way. Never directly use the DOM again.”_ There’s a lot of benefits that a developer gets by using React.

But React is easy to make complex. It’s so very tempting to optimize early. There are a _lot_ of decisions left to you on how to architect your web app.

The techniques listed here will often make your React app have less work to do, and therefore run faster. Implicit performance is a benefit if it means less time later adding optimizations to claw back to a fast experience.

But the primary aim is to make your app easier to write, easier to change, and easier to debug. Less overhead for the person maintaining the code. Writing low overhead React means you can more easily focus on creating that great experience for your users, while not sacrificing the developer experience.

## Low Overhead React Principles

Don’t abandon CSS
Choose packages carefully
Ensure every component does at least one thing well
Don’t let every component talk with the world
Don’t attempt to optimize React rendering too early

## Don’t abandon CSS

Look, CSS (or SCSS) has its limitations. JavaScript is obviously more capable as a language. But CSS is proven for its task. It is fast. It can declaratively handle focus and active and hover states, and it can handle a variety of media queries. It just works with server rendering, and is interoperable with technology choices other than React.

Use CSS variables to create reusable styles for typography, spacing, colors, and other things that are consistent across components.

```css
html {
  --link-color: orange;
}

main a {
  color: var(--link-color);
}
```

You override CSS variables when rendering by using the style prop:

```css
<article style={{ "--link-color": "purple" }}>
  …
</article>
```

(You can see an example like the above here: https://codesandbox.io/s/react-css-variables-chbsj)

CSS is still evolving. The standards are adding more capabilities. There are new approaches to organizing and reusability. Not to mention layout.

If you use CSS-in-JS, it doesn’t mean you are banned from using standalone CSS. Base CSS files work _great_ for reusable styles across components.

## Choose packages carefully

I’ll be honest: there are a lot of heavy packages out there. Calendars. Themeable kits that come with every UI control you can imagine. State and data structure libraries. Many of these dependencies have a big impact on the bundle size, and therefore the time your users will spend waiting for your application.

This doesn’t mean stay from them. Use some, but don’t just add all your first choices. Don’t say yes unless it really provides key value. A package that makes your experience better as a developer (let the team finish faster!) might make your users’ experience worse (let the user start slower!).

Yes, tree shaking can help improve the initial load time, but if you have large packages that you use as the foundation of your app, most of the tree will remain no matter how hard you shake it. And if the shaken out parts are soon downloaded after anyway, you might be consuming a lot of your users’ limited data. We don’t need to pretend NPM doesn’t exist. But we do need to remember that every npm add is also often adding wait time for every user.

## Ensure every component does at least one thing well

Presentation vs Controller. Smart vs Dumb. Connected vs Reusable. Organism vs Molecule. There are many ways of categorizing and splitting-up components into specific roles. I’m not going to offer an opinion here because many ways work well. What I am going to ask is to make sure every component has a role it does particularly well.

You might decide this means the single responsibility principle, so that a component can only be purely HTML focused, or purely subscriber focused, or purely Redux connect focused, or purely analytics focused, etc etc. I leave that up to you, because there often is value in a component doing multiple things. If a component is a function, then it serve to reason that it can be made out of multiple other functions, and these functions can have a single responsibility.

All I request is that you ensure each component does at least one thing particularly well. For example: It’s great at producing markup. It’s great at talking to the rest of the app. It’s great at optimizing render times for this particular case. It may well do more, but at the least it is competent in some particular thing.

If it’s not doing a great job at anything in particular, if it’s trying to speak HTML and API and Redux and URL all at the same time, then it might be time to split it up. A dedicated smaller component can focus on a particular task and communicate its intent better. It’s often fine to keep it in the same file.

## Don’t let every component talk with the world

A component is easiest to manage when it follows a simple contract: given some input, I will return what I want presented. Different input, different output. No other factors.

A slightly more complicated component contract is that: in addition, I will produce some side effects. I will POST to a server, I will talk to a local data store, I will add an event listener directly to the DOM.

Another more complicated variation is I will read from outside of my props. I will fetch from a server, I will subscribe to the latest changes to a local data store, I will listen to when the window changes size.

These components can get out hand. Many components start talking to many things. When hundreds of components have a direct connection with the local data store or communicate with the server. Or when the user resizes the window and many dozens of components individually want to update themselves.

I’m not advocating for a particular architecture or a particular number of communication points or set a limit to how many components are allowed to talk to the server. I’m saying be mindful of how many **connections** there are between components and their outside world. Otherwise a couple of simple changes in the outside world can schedule a huge amount of work.

This may not only lead to performance bottlenecks, but it will also make debugging and future changes harder. Which component made that change? If I rename or restructure this reducer, how many components had this knowledge and so will need to be updated? It’s best to make these questions easier to answer by reducing the surface area of connections.

Make smaller worlds for your components by bundling collaborators together, and then export a simpler component that will be used to hide the details and nuance inside. Add single connection points that bridge between one world and another. Add facades with small surface areas your components see, that then go and talk to the larger world, translating back and forth. Don’t just let any random component talk to whatever it likes. It might feel convenient now, but it’ll likely become inconvenient when changes are needed.

## Don’t attempt to optimize React rendering too early

Don’t optimize your components until there’s a clear problem to be solved. React is designed so that straight forwardly written components will lead to great performance!

If do you want to apply optimizations, first read the documentation provided:

Read the React Docs: https://reactjs.org/docs/getting-started.html
Read the Hooks FAQ: https://reactjs.org/docs/hooks-faq.html
Read React core team-member Dan Abramov’s blog: https://overreacted.io
If these don’t answer your question, then reach out to people from the React core team like [Dan Abramov who happily answers people’s questions on Twitter](https://twitter.com/dan_abramov).

Next, measure using the [open source React DevTools](https://github.com/facebook/react-devtools) which is available for [Firefox](https://addons.mozilla.org/firefox/addon/react-devtools/) and [Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi).

Measure the current profile by clicking record, performing the particular flow that you wish to make more performant, then stopping the record. Use the interactive charts to see the bottle necks and which components need attention.

Make changes, and profile again. It may take several iterations because **your expected optimization technique might make things slower**.

If you have existing optimizations like `React.memo` in place, it might be worth removing them all, and then using the React DevTools profiler to introduce them where you see a meaningful improvement.

Using `React.memo` relies on every prop passed to a component staying the same (being referentially equal) across renders to skip its own rendering. This can be infectious, leading to all your components having to follow this extra contract on their props coming in. Trying to make everything referentially equal can be a lot more computational work that can lead to other performance overheads. And it’s very easy to pass just a single prop that does not stay consistent (say an arrow function), leading to the intended optimization being never applied.

----

React. It’s unique selling point is that it makes writing web apps easier. Yet it’s easy with React to find yourself battling complexity, or for users to be getting penalised performance. Being mindful about the overhead that some techniques and third party packages bring helps keep complexity low, performance easy to find, and future changes maintainable.
