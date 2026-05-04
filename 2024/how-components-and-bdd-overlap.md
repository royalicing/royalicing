---
date: 2024-09-17
substack: https://pgwsmith.substack.com/p/how-components-and-behavior-driven
---
<meta property="og:published_time" content="2024-09-17">

# How Components and Behavior-Driven Tests Overlap

## They are contracts of _what_ and some other system works out _how_.

I write my tests to expect observable behavior instead of mocking internal parts out. They test the **_what_** not the **_how_**. A component takes a similar lens, describing the **_what_** only. The **_how_** is left up to the engine.

Some examples of engines are React, LiveView, HTML/CSS renderer, Terraform, even your printer’s driver is an engine: it works how vector graphics are transformed into targeted deposits of ink.

As you adopt components (using the term ‘component’ broadly for a declarative, composable system) your tests become easier to reason about. Consider your test as a root component testing **Input** -> **Final Output**. And the implementation as little components of **Micro Input** -> **Micro Output**.

An example of a “Final Output” is ARIA-compliant HTML. Your test expects a named **button** and particular **links** and a labelled **combobox** to be on the page. It doesn’t care how they got there.

Theoretically this test could even be used to generate an implementation. For now, human programmers write implementations mostly using off-the-shelf libraries. But I can imagine a future where a practitioner is well versed in writing contracts — e.g. for a UI they would have deep knowledge of ARIA, screen readers, and UX principles — and the implementation is drafted via a large-language model and open source components.
