---
date: 2023-12-29
---
<meta property="og:published_time" content="2023-12-29">

# WebAssembly Core, Imperative Shell

Outline:

- https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell
- Write the core in WebAssembly. This can be functional or object-oriented where you treat the WebAssembly module as a class.
- Write the shell in whatever language you please. This acts as host by instantiating the WebAssembly module.
- The WebAssembly is self-contained with its own state. An interface to the host is imported in as values and callback functions. This enforced interface makes it ideal for writing tests: given these initial values and these stubbed callbacks, expect this output. This contract is what the host is expected to provide.
- The host can also easily stub out the WebAssembly core with a hard-coded one, making testing and prototyping easier.
- The predictable determinism of WebAssembly (given the same input, the exact same result will be outputted) makes adding caches easier. The host can call on WebAssembly for a heavy-duty calculation, and then cache that result for future work.

The WebAssembly core does the following tasks:

- Generating a string output based on input values e.g. rendering HTML/XML/JSON, localization, formatting, URLs.
- Calculate primitive integer & boolean values e.g. math, permissions, time offsets.
- Output values conforming to a protocol e.g. HTTP status/headers, SQL, Redis commands.
- Parse or validate input e.g. URL search params, form data, JSON.

The host does the following tasks:

- Communicate over the network e.g. HTTP requests & responses, databases, WebSockets, DNS, external APIs.
- Encode/decode standard formats like compression
- Manage/invalidate caches.
- Instantiate, coordinate, and destroy WebAssembly instances.

The host can be written in proven languages and battle-hardened frameworks. The many years of investment means best-of-breed HTTP libraries, database drivers, and application toolsets exist in many forms.

The WebAssembly core can be executed in an isolated sandbox, preventing many attacks from even being possible.
