---
date: 2023-03-27
---
<meta property="og:published_time" content="2023-03-27">

# Comparing HTTP Latency

When an HTTP request is made, a response comes back, right? But how long does that response take to come back? And does it come back all in one go, or do the headers and status code come first and then the body?

These things affect the user experience, whether something feels fast or slow to a user. And so I thought it worth building a tool that lets you compare HTTP latency, and even enter your own URL to see how you fare.

## How it was made

- HTML form
- Tailwind CSS
- Phoenix LiveView
- HTTP requests using Mint. Requested serially as a form of backpressure.
