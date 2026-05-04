---
date: 2023-03-28
---
<meta property="og:published_time" content="2023-03-28">

# Introducing Collected Press: Markdown Directly Loaded from GitHub, Rendered on the Edge

For a while, I’ve had a blog running on WordPress. I chose WordPress because I wanted something simple. I didn’t want to use a React framework or a static site generator, as from experience those things get out of date and then you have to migrate every year or so to the new version.

WordPress became painful, and not just because there’s a new plugin update every time I log in, but more because the [Gutenberg block editor](https://wordpress.org/documentation/article/wordpress-block-editor/) is actually more work for me than just writing Markdown, especially when adding code snippets. I want to have JavaScript and CSS snippets together in the same blog post and have them highlighted correctly. This required some custom code in my custom theme and it all got a bit much.

So of course I’ve done the dumb thing and written my own blog system. But there’s two things about it I think make it worthwhile. The first is all the content lives in a GitHub repo in a simple file/folder structure. The second is the content is fetched on-the-fly and served on the edge (using Cloudflare Workers).

That’s right, on the fly. Every time you load this page, I’m asking GitHub for the current HEAD of my github.com/RoyalIcing/RoyalIcing repo and then using that SHA and requested URL path to lookup the matching content from `raw.githubusercontent.com`. 

## Buildless

There’s no build step or GitHub action. There’s nothing wrong with having these, except I often find that they too get out of date, break, and then need attention. I just wanted to write a new post, and I ended up spending an afternoon reading release notes, updating dependencies, and fixing bugs.

Instead I want to be able to create a new file, write some Markdown in it, and have it appear as a new blog post or web page without fuss. I especially want to be able to do this from my phone, so if I see a typo and want to fix it, or if I come up with a new post idea while away from my laptop, I want to be able to do it with the device I always have at hand.

This means I don’t want to debug from my phone. I don’t want to be scrolling through some build pipeline’s output, and study what version of Node.js it’s using, and see what commands it ran and then which one hit a snag. I want to write some Markdown and then it appear online without a hiccup. GitHub can do [something like this](https://pages.github.com) but I don’t want to worry about Ruby or Node.js. Ideally GitHub and other services copy the idea of Collected Press and serve content on-the-fly from repositories.

## Syntax highlighting on the server

I also want syntax highlighting performed on the server. The React community is coming around to doing things on the server (such as server components), but in my opinion it took far too long to get here and the community has historically just outsourced everything to the user’s browser leading to long loading times and thirsty battery use. Collected Press does all the Markdown-to-HTML conversion and syntax highlighting on the server, and then runs zero client-side JavaScript. This ensures a fast experience no matter what device readers are using.

These code snippets are syntax highlighted on the server using [highlight.js](https://highlightjs.org), with a small `night-owl.css` loaded by the browser from `unpkg.com` to add styling. I don’t have to detect which languages are used and only load their syntaxes, I can have them all available on the server.

```html
<button id="hello">Some example HTML</button>
```

```jsx
export function Example() {
    return <Button id="hello">Some example React</Button>;
}
```

```css
:root {
    font-size: calc(100% + 8vw);
}
```

## Deploy speed

I haven’t seen anything else with the deploy speed, where I `git push` my Markdown file and then switch to my production site and reload and the new page is instantly there. That’s the power of having no build step. There’s a confidence gained in that short period of unease where you wonder if something got tripped up in the build just disappearing. It should feel the same as posting to social media, which just works.

I want to be able to spin up new sites without having to worry about managing a theme or database or keeping some popular JavaScript tool up-to-date. Collected Press does this for me, and perhaps it might for you too.

## Proxying of CDNs

It’s a good idea to [reduce the number of DNS lookups](https://csswizardry.com/2013/01/front-end-performance-for-web-designers-and-front-end-developers/#section:http-requests-and-dns-lookups), so instead of linking out to unpkg.com directly, I proxy `unpkg.com` requests via a subpath like `/unpkg.com/modern-normalize@1.1.0/modern-normalize.css`, fetching it on the edge and then forwarding its response back. I do the same for highlight.js’s CSS.

In the Worker this looks like:

```js
const allowedUnpkgPackages = ['modern-normalize', 'highlight.js'];

export default {
  async fetch(request, env, ctx) {
    const url = new URL(request.url);

    if (allowedUnpkgPackages.some(name => url.pathname.startsWith(`/unpkg.com/${name}@`))) {
      return fetch(`https:/${url.pathname}`, { cf: { cacheEverything: true } });
    }

    …
  }
};
```

I’m not sure whether to add this as part of the Collected Press package itself, as I want to keep that pretty focused. The cool thing about the library effectively just being a function that maps a `Request` into `Response` is that you can compose multiple of these functions together.

## Dynamic footer

One of these functions uses Cloudflare’s [`HTMLRewriter`](https://developers.cloudflare.com/workers/runtime-apis/html-rewriter/) to add link to the source repo’s SHA that was loaded.

```js
function addSHAToResponse(res, sha) {
  return new HTMLRewriter()
    .on('footer[role="contentinfo"] p', {
      element: (element) => {
        element.after(`<p><a href="https://github.com/RoyalIcing/RoyalIcing/tree/${sha}"><small>SHA: ${sha}</small></a></p>`, { html: true },)
      },
    })
    .transform(res);
}

export default {
  async fetch(request, env, ctx) {
    const source = sourceFromGitHubRepo('RoyalIcing', 'RoyalIcing');
    const sha = await source.fetchHeadSHA();
    const res = await source.serveURL(url, { commitSHA: sha });

    return addSHAToResponse(res, sha);
  }
}
```

## Caching

When listing the most recent blog posts, this requires a lot of requests loading the Markdown for every single article to extract its title and publication date (which is used to sort the posts). This makes loading these index pages a little slow.

I’ve used Cloudflare’s KV store to cache rendered HTML content for a particular SHA and URL path. As Git commits are immutable, the content for a particular SHA is always the same. We can take advantage of this immutability to confidently cache forever. When the repo receives a new commit, we’ll see a new HEAD SHA which means we’ll load fresh content.

I’m experimenting with a [stale-while-revalidate](https://web.dev/stale-while-revalidate/) strategy to caching, to try to make response times as quick as possible. Cloudflare’s `ctx.waitUntil()` is used to perform some work in the background, unblocking the response back to the user. The code for it looks like:

```js
let lastKnownSHA = await env.swr_cache.get(cacheKeys.headSHA);

const headSHAPromise = source.fetchHeadSHA();
ctx.waitUntil(headSHAPromise.then(headSHA => env.swr_cache.put(cacheKeys.headSHA, headSHA)));

if (lastKnownSHA == null) {
    lastKnownSHA = await headSHAPromise;
}
```

## Other things

- If I wasn’t to come up with Collected Press, I would likely have chosen Astro. I love the HTML-first and server-first approach, and I love the range of integrations. Perhaps there’s a way to make Collected Press work within Astro?
- The styles for my site are in single `<style>` tag in the `<head>`. However, you can add `.css` files or images and have those be served up.
- I initially had a single server that could load from any GitHub repository, which mean that my site’s separate Cloudflare Worker could do everything just with `fetch()` to that server. But it felt too risky if others also relied on that service and then updating it would feel as fraught as running one of the popular Mastodon servers. So instead I’ve made a little NPM package that you deploy to Cloudflare
- I’ve tried deploying it to Deno Deploy too, but I seem to be having trouble with ES modules. This is the sort of crap I‘m trying to get away from — it’s not Deno’s fault but it’s an issue stemming from the switch from `require()` to `import/export` and JavaScript’s now having two modes that must be managed correctly. Ideally I’d rewrite the `@collected/press-server` package to have no dependencies but that’s a lot more work, so instead I’ve stuffed all that pain into this single package-sized box which hopefully only need occassional updates.
- Even though the Markdown content and the Cloudflare Worker code live in the same Git repo, there’s actually two concepts of a deploy. The first is for deploying the Worker, which simply imports the package and runs a function telling it which GitHub repo to load content from. The second is when pushing content to GitHub, which updates the `HEAD` of my main branch, which means the Worker will load content for that new Git SHA. So because these’s two things are completely separate, there’s no requirement that they live in the same repo.
