---
date: 2020-08-20
---
<meta property="og:published_time" content="2020-08-20">

# My most used commands for front-end

```bash
# Measure HTTP response time
httpstat "https://example.org/"

# Run Google’s PageSpeed Insights
npx psi "https://example.org/" --strategy=mobile

# Run Axe Core across multiple breakpoints
npx evaluatory "https://example.org/"

# Optimize & minify SVG file
npx svgo <file.svg>

# Visualize JavaScript usage
npx bundle-wizard example.org

# Create a new TypeScript library with TSDX
npx tsdx create <project-name>

# Create a Remix app
npx create-remix@latest

# Create a new web app with Vite
npm init vite-app <project-name> # Vue by default
npm init vite-app <project-name> --template preact
npm init vite-app <project-name> --template react

# Creates a Cloudflare Worker
npm i -g @cloudflare/wrangler
wrangler generate <project-name>
```
