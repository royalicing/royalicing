---
date: 2019-03-27
---
<meta property="og:published_time" content="2019-03-27">

# Handy SPA config for Netlify

Although Netlify out of the box won’t work with single page apps’ routing, it is easy to add. You can configure it to send all `/*` subpath requests to a single `index.html` file.

## Steps for create-react-app

Here we will be using a **create-react-app** project. This should be easy to apply to other SPA toolkits.

Add a `netlify.toml` file to your `src` folder. In it write:

```toml
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

Edit your `package.json` file. Find the `scripts` section.

```json
  "scripts": {
    …
    "build": "react-scripts build",
    …
  }
```

Replace the `build` rule with three:

```json
    "build": "npm run build:react && npm run build:config",
    "build:react": "react-scripts build",
    "build:config": "cp src/netlify.toml build/netlify.toml",
```

This rule builds the static assets using create-react-app’s toolkit, then copies the `netlify.toml` file to this `build` directory.

That’s it! As long as Netlify is set to run `npm run build` then this will work. You will be able to visit subpaths and get the same index.html page served for all.
