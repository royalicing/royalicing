---
date: 2018-12-17
---
<meta property="og:published_time" content="2018-12-17">

# Understanding state

State is what is known by your app.

In a web app, state refers to local state. It only knows a subset of all information. And that information is from a certain point in time: it might have changed since.

In a JavaScript web app, your user’s web browser talks to an API. The API has its own state, usually stored permanently in a database.

(In a server-side web app like one built with Rails, your application usually talks to the database directly.)

So local state. It’s your web app’s state of the world at a certain time. That world depends on factors, such as which account is signed in, or perhaps whether the user is not signed in, which page they are looking at, and what capabilities their account has.

## Loading state

For your web app to display information, it has to first load it.

So your web app loads this initial information, and this becomes its state of the world. If five minutes later, the latest information is desired, it must load it again.

Making things more complicated is the fact that there is not just one set of information.

Take Instagram as an example. There is the feed of photos. As you scroll through the feed, more photos are loaded. For a good user experience, photos are loaded before you reach them. But still, only a subset of photos are loaded. It would make little sense to load your feed from the top to the very bottom, as it would likely be hundreds of megabytes of information in total, and be a huge strain on Instagram’s servers to collate all that information.

Instagram offers more than just a feed. Your can explore and search for photos. You can take or upload a photo. You can see a list of activity aimed at you, such as recent comments or likes on your posts. You can also look at your own profile, with the photos you have posted.

Depending on which section you look at, and how you interact within that section (scroll, tap to see details, tap to go to someone’s profile), new information will have to be loaded. The app’s state of the world expands. It goes from a small subset of data, to a larger subset of data.

Managing this state, and coordinating the loading of additional or fresher state, is one of the key skills in building web apps.

## A step back. A command line app.

Any app that has an interactive user interface has state. Think of PowerPoint and the file that is being viewed, the currently active slide, whether that slide is being edited, viewed with slides listed to the side, or is being presented. All of those variables are state.

A command line app also has state. The less command efficiently reads from a file, only presenting a slice of it that fits in the terminal window. As the user scrolls and down, the app’s state is updated with the offset slice into the file.

A command line app is a useful starting point, because many operate in the same manner as web apps based on React. In React, the application is built by deriving the entire user interface from state. The application developer declares their intent for what should be displayed on screen given a certain state. If the state changes, then that codified intent is used again, but with the updated state. And so on, every user interaction usually affects the state in some way, and the user interface is updated quickly to respond. This pipeline approach is popular with games, where the displayed image is rebuilt again and again many times a second, fast enough to produce fluid motion.

The alternative is to intertwine the state with what is being presented. As new information comes in, it is not a simple change to state. It is a manual change to what’s on screen too.

A pipeline is like to correct a typo in a printed document, making the change in software, throwing the old copy away, and printing a new fresh copy. Fortunately, in an app built totally with software means that nothing is wasted.

An intertwined approach is more akin to correcting a typo by using whiteout and a fine pen. It’s much less wasteful, but takes much more effort and skill than just printing a new copy would be. The same is true for apps.
