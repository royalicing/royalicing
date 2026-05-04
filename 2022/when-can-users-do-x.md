---
date: 2022-05-24
---
<meta property="og:published_time" content="2022-05-24">

# When Can Users Do X?

Lighthouse and Core Web Vitals have brought concepts such as Largest Contentful Paint and Time to Interactive into many front-end developer’s lingo.

However, these three-letter-acronyms are easy to get confused with each other. What’s CLS again? What’s the difference between FID and TTI?

I’d like to propose an alternative language, from the user’s experience:

## When Can Users Do X?

1. User starts loading
2. User can see something
3. User can hear something
4 User can read
5. User can scroll
6. User can zoom
7. User can interact
    - User _thinks_ they can interact
    - User _knows_ they can interact (from previous experience)
    - User _actually_ successful interacted
8. User can share
9. User can close

This potentially goes broader than a particular browser life-cycle event. It starts to ask questions like “what’s the user’s goal in visiting this page?”, is it to read themselves (to read a recipe), or to share the link with someone else (to share the page to a booking with a partner)?

Perhaps the user tried zooming, but because the page was still loading then content moved around like a new born calf on a trampoline. So you’d say that the user *couldn’t* zoom until layout stopped shifting. I’m thinking less in terms of “Cumulative Layout Shift” and more in terms of “some users like to zoom, so when can they start doing that?”

And it begins to break the assumption that all users read the page by seeing, but some rather read by hearing. As great as tools like Lighthouse have been, they sort of assume the user is interacting with the page visually.
