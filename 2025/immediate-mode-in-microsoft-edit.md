---
date: 2025-05-22
---
<meta property="og:published_time" content="2025-05-22">

# Immediate mode in Microsoft’s new Edit TUI

Microsoft released a new open source code editor. Instead of being a complex, mega-dependency Electron app, it’s a Rust [terminal-user-interface](https://en.wikipedia.org/wiki/Text-based_user_interface) with minimal dependencies.

The essay at the top of their TUI source code is fantastic by itself: <https://github.com/microsoft/edit/blob/bf5178b44cc8c11f9aa7130a5c834d1e3ce07649/src/tui.rs>

> An immediate mode UI framework for terminals.
>
> ## Why immediate mode?
>
> This uses an “immediate mode” design, similar to [ImGui](https://github.com/ocornut/imgui). The reason for this is that I expect the UI needs for any terminal application to be fairly minimal, and for that purpose an immediate mode design is much simpler to use.
>
> So what’s “immediate mode”? The primary alternative is called “retained mode”. The difference is that when you create a button in this framework in one frame, and you stop telling this framework in the next frame, the button will vanish. When you use a regular retained mode UI framework, you create the button once, set up callbacks for when it is clicked, and then stop worrying about it.
>
> The downside of immediate mode is that your UI code _may_ become cluttered. The upside however is that you cannot leak UI elements, you don’t need to worry about lifetimes nor callbacks, and that simple UIs are simple to write.
>
> More importantly though, the primary reason for this is that the lack of callbacks means we can use this design across a plain C ABI, which we’ll need once plugins come into play. GTK’s `g_signal_connect` shows that the alternative can be rather cumbersome.

The [DOM uses](https://web.dev/articles/howbrowserswork) retained mode. The browser takes the DOM element tree and from it creates a retained layout tree, which then maps to a retained render tree that is used to paint pixels to the screen.

React components give you an immediate mode API on top of all of this, which then has its own retained tree that it diffs to mutate the DOM.

Phoenix LiveView has a similar immediate mode API, where your render function declares what the current HTML should be for a given set of assigns. It server-side code sends the diffs over the network so its other client-side arm can mutate the DOM.

The GPU is also retained mode, where the CPU uploads vertex and fragments to the GPU (this is why you must wait for games to load, you are waiting for 2D/3D models & textures to travel from disk to CPU to GPU).

Another way of looking at it is the retained mode trees are complex caches that must all be synced in alignment. If you took out the middlemen you’d have a much simpler programming paradigm and a more efficient runtime.

If the GPU is the only actually-required retained mode tree, and if a single immediate mode layer on top is plenty fast it might be the ideal mental & system model.
