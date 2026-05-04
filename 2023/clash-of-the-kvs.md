---
date: 2023-05-01
---
<meta property="og:published_time" content="2023-05-01">

# Clash of the KVs

Today [Vercel announced](https://vercel.com/blog/vercel-storage) a range of storage options, from Postgres to an S3-like object store. One of those is a hosted Redis offering called [Vercel KV](https://vercel.com/docs/storage/vercel-kv#how-vercel-kv-works).

This would be fine if there weren't two other offerings from competitors also called KV.

First, we have [Cloudflare KV](https://developers.cloudflare.com/workers/learning/how-kv-works/) which was released a few years ago. This lets you work with key-value pairs, which are replicated and cached around the world. It's optimized for read-heavy workloads, where you'll have Cloudflare Workers spinning up all around the world and you want them to see the same config. It only guarantees eventual consistency though, so if you write a value to a key you might see stale values for some period after.

Then we have [Deno KV](https://deno.com/kv), which just to make things more confusing has two flavors. The first is backed by SQLite, and the second by FoundationDB ([Deno Deploy KV](https://deno.com/deploy/docs/kv)). This offers much better stronger guarantees of consistency, with linearizability (after you write a value, all following reads will see that value) and serializability (you can have multiple transactions that atomically see and change the data).

So just Cloudflare KV and Deno KV alone sit on the opposite sides of the data consistency spectrum. And then comes Vercel KV, which is a re-packaged version of Upstash, offering hosted Redis with persistence to disk. Redis offers some atomic operations, but becomes eventually consistent if you enable replicas.

So we have 3 different modern cloud storage services, all called KV but offering completely different feature sets and guarantees around data consistency. The appeal around the term "key value storage" is clear, as reading and writing to a key is a very simple concept for devs to pick up. But I think at least one of these services ought to change their name, to help differentiate that they are more than just a lowest-common-denominator Memcache clone.
