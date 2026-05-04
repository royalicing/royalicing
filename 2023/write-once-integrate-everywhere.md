---
date: 2023-03-15
---
<meta property="og:published_time" content="2023-03-15">

# Write Once, Integrate Everywhere

Write once, run anywhere clearly fails. Every platform and device is different, so I believe if you write for the lowest common denominator then you’ll get a lowest common denominator experience.

It can work (see [Visual Studio Code][vscode] or [Blender][blender] for some great apps), but the typical cross-platform app is just not as good as one made with the target platform in mind.

It’s why initiatives such as [Mac Catalyst](https://developer.apple.com/mac-catalyst/) sadden me as they feel like pasting French poetry into Google Translate and expecting the automated English to read as beautifully as the original. Translating poetry is [its own art form](https://www.readpoetry.com/poetry-translation/):

> There’s a reason translators of poetry are usually also poets themselves — they are capturing something implicit in the poem. They look for a zeitgeist, a feeling, and they find equal and opposite meanings to bridge linguistic and cultural gaps. But this is no easy feat.

One place where ‘write once, run anywhere’ does work is on the web. You can have the same HTML and the same CSS and have it adapt to the particular viewport size and device capabilities the user has access to.

But while wide compatibility is a strength of the web, it’s also a weakness. Web browser engines have gotten so full of features they are basically operating systems. Windows XP was [45 million lines of code](https://www.facebook.com/windows/posts/thanks-to-everyone-who-participated-in-todays-round-of-trivia-windows-xp-was-com/155741344475532/), WebKit now has [24 million lines](https://www.openhub.net/p/WebKit), and Chromium has [35 million lines](https://www.openhub.net/p/chrome/analyses/latest/languages_summary). So when you run an app [built upon Electron](https://www.electronjs.org/blog/10-years-of-electron) to me it’s about as sensible a use of computing resources as running a Windows app on your MacBook using the [Parallels emulator](https://www.parallels.com/).

Is there another option? On one side it seems we have artisal software hand-crafted with the tools the platform-maker intended, and on the other we have mass-produced software that abstracts the underlying platforms as featureless vessels.

These two opposing points create a spectrum, and we can choose multiple points along that spectrum to balance the trade-offs between reuse and polish.

A better user experience might require using the platforms’ native tooling for the user interface. But a data storage system might benefit from putting the wood behind one arrow, increasing its robustness by allowing a single system to be tested, also improving the end user experience.

1Password decided this was [the right approach for their apps](https://blog.1password.com/1password-8-the-story-so-far/) which run on Macs, PCs, iPhone, iPad, Android, Linux, and the web:

> The goal was to put every feasible piece of 1Password into the Core library, stopping just short of the user interface. This approach has allowed us to consolidate everything from the communication with the 1Password.com server, to the database handling, to permissions enforcement, to our cryptographic routines, and more in one place. It’s also allowed us to drive the consistency of user experience we need.

JSON is an example of write once, integrate anywhere. Every programming language has a way to take a JSON string and decode it into a data structure. Most also have a built-in way to make HTTP requests, and so RESTful JSON API servers are a natural fit in many app stacks.

JavaScript has attempted to be the universal language that will run everywhere, but I believe its ecosystem is complicated enough that it’s not fit for that purpose.

WebAssembly shows more promise. It not only runs in all web browsers today, but can run on all mobile devices, laptops, desktops, and servers. In the same way you can give a browser a `<script>` tag and it‘ll download and execute the JavaScript no matter where it is hosted on the internet (that’s how CDNs work), you can download and execute a WebAssembly module on the fly.


[vscode]: https://code.visualstudio.com
[blender]: https://www.blender.org
