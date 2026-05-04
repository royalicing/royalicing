---
date: 2023-03-18
---
<meta property="og:published_time" content="2023-03-18">

# An Idea for Figures in Markdown

Markdown has a built-in way to add images. You write:

```markdown
![text describing the image](https://yourserver.net/link-to-image.png)
```

And the HTML produced looks like:

```html
<p><img src="https://yourserver.net/link-to-image.png" alt="text describing the image"></p>
```

I particular like how `alt` support is there by default, encouraging writers to add an accessible description of the image screen reader users (or web crawlers) can read.

However, HTML evolves, and one of the additions that came with HTML5 is `<figure>`. This is an element borrowed from printed works, especially in non-fiction where they’ll for example say “see figure 1.4 for an illustration of a plant cell”.

I first encountered `<figure>` in A Book Apart’s [_HTML5 for Web Designers_](https://abookapart.com/products/html5-for-web-designers) whose first edition came out in 2010, well over a decade ago. So it’s not exactly new.

One benefit of `<figure>` over a standalone `<img>` is that the descriptive text is visible by all in the form of a caption. So if you were writing the HTML by hand, instead of:

```html
<p><img src="https://yourserver.net/link-to-image.png" alt="text describing the image"></p>
```

you’d write:

```html
<figure>
    <img src="https://yourserver.net/link-to-image.png" alt="">
    <figcaption>text describing the image</figcaption>
</figure>
```

---

## An idea for a figure Markdown syntax

So how would you model such a thing in Markdown? There are plugins such as [markdown-it-image-figures](https://github.com/Antonio-Laguna/markdown-it-image-figures) which let you use the existing `![]()` image syntax and allow them all to be rendered with wrapped `<figure>` elements instead. But this requires using a particular plugin, which may work with `markdown-it` but not another Markdown library. And it changes all images to become wrapped in figures, I’d prefer to be explicit so existing images keep working as originally intended.

I ideally want something future proof that will be supported no matter if I use a Markdown library written in JavaScript, Perl, or Rust.

Here’s my proposed syntax, extending the existing image syntax slightly.

```markdown
!1[text captioning the image](https://yourserver.net/link-to-image.png)
```

```html
<figure id="figure-1">
    <img src="https://yourserver.net/link-to-image.png" alt="">
    <figcaption>text captioning the image</figcaption>
</figure>
```

You can add any digit or hypen-separated digit sequence:

```markdown
!42[text captioning the image](https://yourserver.net/link-to-image.png)
!1-23[text captioning the image](https://yourserver.net/link-to-image.png)
!1-23-45[text captioning the image](https://yourserver.net/link-to-image.png)
```

```html
<figure id="figure-42">
    <img src="https://yourserver.net/link-to-image.png" alt="">
    <figcaption>text captioning the image</figcaption>
</figure>

<figure id="figure-1-23">
    <img src="https://yourserver.net/link-to-image.png" alt="">
    <figcaption>text captioning the image</figcaption>
</figure>

<figure id="figure-1-23-45">
    <img src="https://yourserver.net/link-to-image.png" alt="">
    <figcaption>text captioning the image</figcaption>
</figure>
```

The `id` attribute is added so that you can link to it with a fragment:

```markdown
See [figure 1.23 for an illustration](#figure-1-23) of the plant cell.
```

```html
<p>See <a href="#figure-1-23">figure 1.23 for an illustration</a> of the plant cell.</p>
```

If you wish to include the figure identifier in the caption, you must write that out yourself.

```markdown
!1-23[Figure 1.23: an illustation detailing the parts of a plant cell.](https://yourserver.net/link-to-image.png)
```

The slight addition of a digit to the existing image syntax lets existing Markdown writers learn it without switching to an entirely new syntax. It’s also unlikely to clash with existing written Markdown, as I believe `!:digit:[` would be an odd mix to be already present.

---

## Unanswered questions:

- How to support adding links? Is the image wrapped in a link, the text within the caption, or the entire `<a href="…"><figure>…</a>` is wrapped in a link?
- How to support multiple articles on the same page that each have a `id="figure-1"`? How does Markdown currently handle this with footnote links?
- Backwards compatibility. Currently writing `!42[text captioning the image](https://yourserver.net/link-to-image.png)` would produce the text `!42` followed by a hyperlink to the image.
- Are only digits and hyphens supported? Could you support alphabetical characters, say to give a figure a memorable `id` for fragment hyperlinks?
