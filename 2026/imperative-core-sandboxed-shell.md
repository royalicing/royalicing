---
date: 2026-01-08
---
<meta property="og:published_time" content="2026-01-08">

# Imperative Core, Sandboxed Shell

What if instead of implementing using functional programming, which often copies data that has to be later collected, we wrote code that matched the operation & memory model of the underlying compute?

And then put that inside a strong sandbox?

That’s how Docker works, where it can run any number of living processes isolated from the host OS.

That’s how GPU shaders work, where an imperative function is run for each pixel.

That’s how WebAssembly works, where every input & output is explicit to a virtual machine.

With coding agents why don’t we leverage sandboxing? Why can’t they generate programs that use compute & memory optimally? Why can’t we provide them something much lighter than headless Chrome to quickly iterate with? Why not lean on their strength of porting between languages?
