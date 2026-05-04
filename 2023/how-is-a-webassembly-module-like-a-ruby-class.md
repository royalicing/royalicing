---
date: 2023-07-19
---
<meta property="og:published_time" content="2023-07-19">

# How is a WebAssembly module like a Ruby class?

You may have heard of WebAssembly. If so, you probably know you can write C or Rust and compile it to WebAssembly. But here’s a different mental model — after exploring WebAssembly the past couple of years, I think it actually looks a lot like a Ruby class.

## WebAssembly modules ~= Ruby classes

First is the WebAssembly module itself. Each `.wasm` file is its own WebAssembly module.

The WebAssembly module itself is an inert definition — just like a class. To actually run a WebAssembly module you must instantiate it. A Ruby class becomes and object, and a WebAssembly module becomes an instance.

## WebAssembly instances ~= Ruby objects

And like a class, you can instantiate it multiple times. One WebAssembly module can become multiple WebAssembly instances.

## WebAssembly imports ~= Ruby initializers

When you instantiate your Ruby class, you can pass in initial arguments that help bootstrap the object to an `initializer`.

You can do something very similar in WebAssembly: you can import values and functions from the outside world. For example, you could import a function to generate a random number, or you could import a set of functions to execute SQLite queries.

## WebAssembly functions ~= Ruby methods

The use a Ruby object, you send messages to it. These then call methods. The methods can alter the internal state of the object.

The use a WebAssembly instance, you call its exported functions. The functions can alter the internal state of the instance.

## WebAssembly globals ~= Ruby instance variables

In Ruby the internal state of objects are called instance variables.

In WebAssembly the internal state of instances are called globals.

Globals are variables that are defined at the top of a WebAssembly module. Any function can read or mutate them.

Globals can be mutable or readonly. They can be exported to the outside world or stay internal.

This is very similar to a Ruby class’s instance variable. Instance variables live ‘at the top’ of a Ruby object. Any method can read or mutate them.

Because of the similarity, in Orb I’ve used the same syntax for globals as Ruby uses for instance variables: `@global_name`.

----

I hope this cast light on WebAssembly modules as not a foreign concept, but as actually something entirely familiar.

If you are interested in authoring WebAssembly modules, I suggest you check out my [Elixir DSL called Orb](https://hexdocs.pm/orb/Orb.html).
