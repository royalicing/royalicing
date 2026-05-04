---
date: 2023-10-30
---
<meta property="og:published_time" content="2023-10-30">

# Web App vs Site vs Hybrid

I first started as a website designer, and now call myself a web app developer. Although what we can build on the web has become much more capable, I think the distiction is still useful. Even if you have both a blog and a dashboard in the same “web app” you should treat them differently: always server-rendering the blog for example.

## Web site: primarily for reading

- URLs _must_ be shareable and bookmarkable.
- If you change a URL e.g. `/team` to `/about`, then the old URL _must_ 30x redirect to the new URL.
- HTML `<head>` _must_ be server rendered to provide to social networks and communication tools (e.g. Slack & Discord).
- HTML `<body>` _must_ be server rendered to provide to search engine crawlers and read-later services. This means that everything can work in user agents without JavaScript.
- HTML `<body>` _must_ have Aria landmarks, such as `main`, `nav`, `banner`, `form`, `search`.
- If the subroute is invalid (e.g. article not found), then the HTTP status _must_ be 4xx. This ensures search engine crawlers won’t add the URL to its index.
- `LCP`: The largest content should load within 4 seconds on slow 4G.

## Web app: primarily for interacting

- URLs _should_ be shareable and bookmarkable.
- If you change a URL e.g. `/item/456` to `/note/456`, then the old URL _should_ 30x redirect to the new URL.
- HTML `<head>` _should_ be server rendered to provide to social networks and communication tools (e.g. Slack & Discord).
- HTML `<body>` _can_ be browser rendered to provide interactive controls.
- HTML `<body>` _must_ have Aria landmarks, such as `main`, `nav`, `banner`, `form`, `search`.
- Even if the subroute is invalid (e.g. resource not found), the HTTP status _may_ be 2xx. This allows HTTP streaming or JavaScript `fetch()` to come after the HTTP headers have been sent.
- Loading data _must_ work on slow and spotty connections.
    - Server rendering the most important data can improve odds of success.
- Actions & forms _should_ work on slow and spotty connections.
    - Progressively enhancing a standard browser `<form>` submission can improve odds of success.
    - Submitting a form _must_ tell the user if it failed and _should_ offer a way to recover or retry.

## Hybrid: both reading and interacting

Most web apps today are a hybrid. You might have a landing page, pricing page, documentation, a blog, sign up, sign in, and a dashboard with various sections.

Here’s how I’d break these routes down:

### Site: landing, pricing, documentation, blog.

This is content designed to be read and shared by users.

- Will be shared and bookmarked by users.
- Will be found elsewhere by users e.g. via Google search, social media post.
- Must be server rendered HTML. This means the primary content _must not_ be loaded via JavaScript.
- Can be enhanced with JavaScript e.g. interactive charts, playground in docs.

### App: sign in, sign up, dashboard with various sections.

These are screens designed to be interacted (forms, taps, clicks) by users.

- Can be shared by users, probably within a private circle e.g. work chat.
- Can be bookmarked by users.
- Can be browser (instead of server) rendered.
