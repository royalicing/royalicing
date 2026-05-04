---
date: 2023-03-16
title: WebAssembly allows script tags everywhere
---
<meta property="og:published_time" content="2023-03-16">

# WebAssembly allows `<script>` tags everywhere

The web is a flexible medium. You can pull in a CSS or JavaScript resource from any URL onto you HTML web page.

This is both the web’s strength and weakness. Being able to use existing hosted assets and simply link them to use them shows sites to be assembled quickly, reusing proven parts.

But allowing third-party code to affect your experience is a risk, both to your security and your users’. If you link in external CSS or JavaScript, you better have faith you can trust it. 

There’s a new kid in town called WebAssembly. This technology has been designed to be secure, not compromising the host machine running it, while being speedy and powerful. It’s an impressive combination unlocking capabilities that neither CSS nor JavaScript could achieve before.

The mental model of WebAssembly modules is of a black box. You download it, then create an instance passing some initialisation values, and interact with it via explicitly defined exports. But you don’t need to know exactly from the outside how it’s ticking in there.

That’s another strength, as you can write WebAssembly modules in a variety of languages, from Rust to Zig to Golang. You don’t need to know what language a module was authored in to use it (though some, like Golang, do have conventions that have to be followed).

WebAssembly doesn’t just work in browsers. It has a variety of engine implementations that work in native applications, and that work on the server.

This means you can have the same benefits of linked external JavaScript, but on the server. A browser script works by:

1. Downloading the URL using HTTP. 
2. Parsing and executing the script.
3. Integrating with other systems, such as the HTML object-oriented paradigm, the DOM.

A WebAssembly running on the server can work the same way:

1. Download the URL using HTTP. 
2. Parsing and executing the module.
3. Integrating with other systems, such as a database or other backend technologies.

Some examples of use cases are syntax highlighting of code, or transforming Markdown to HTML. These common tasks, once implemented in WebAssembly, can be run interactivity from a browser, safely and quickly on a server, or directly on a mobile device. It’s write once, integrate anywhere.

No longer do we have to find the implementation for our chosen programming language. No longer do we have to find the best version for browsers, the best version for our servers, and the other best version for mobile apps — and then hope they are all compatible. We can use the same version everywhere.
