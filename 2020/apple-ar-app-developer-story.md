---
date: 2020-12-21
---
<meta property="og:published_time" content="2020-12-21">

# The Apple Experience Augmented — Part 2: Developer Experience

Apple has been iterating for years on a curious set of APIs called [ARKit](https://developer.apple.com/augmented-reality/arkit/) that has been largely ignored by the iPhone & iPad developer community, which hints that more compelling hardware is coming to take full advantage. CEO Tim Cook [has openly said](https://vrscout.com/news/apple-tim-cook-ar-next-big-thing/) AR is the ‘next big thing’.

[Rumors suggest](https://www.macrumors.com/roundup/apple-glasses/) Apple is developing an augmented reality (AR) glasses product. This product would likely act as an iPhone periphery, at least initially, similar to how Apple Watch once relied on a host iPhone to provide the main computational grunt. Well informed supply chain analyst Ming-Chi Kuo [has said](https://www.macrumors.com/roundup/apple-glasses/) the AR glasses “will primarily take a display role offloading computing, networking, and positioning to the iPhone.”

Apple has recently introduced M1 Macs powered by Apple Silicon. These Macs are notable because they bring a marked improvement in battery life and performance. But they also bring Apple’s developer devices finally in line with the more capable hardware of their consumer devices.

Apple’s head of software engineering Craig Federigi [talked with Ars Technica](https://arstechnica.com/gadgets/2020/11/we-are-giddy-interviewing-apple-about-its-mac-silicon-revolution/) about the advantages of M1’s unified memory architecture:

> Where old-school GPUs would basically operate on the entire frame at once, we operate on tiles that we can move into extremely fast on-chip memory, and then perform a huge sequence of operations with all the different execution units on that tile. It’s incredibly bandwidth-efficient in a way that these discrete GPUs are not.
> 
> [Ars Technica interview with Apple executives](https://arstechnica.com/gadgets/2020/11/we-are-giddy-interviewing-apple-about-its-mac-silicon-revolution/)

Tiled rendering will be needed by the upcoming AR glasses product, with the increased throughput allowing both high resolutions and high frame rates. High frame rates are required as [“frame rates below 90 frames per second (FPS) is likely to induce disorientation, nausea, and other negative user effects”](https://steantycip.com/blogs/the-importance-of-framerates-in-vr/). The iPad Pro can already achieve 120 frames per second, so it’s likely the AR glasses’ display would reach similar rates.

So how would you develop apps for such a device? Let’s look at how developing software for the iPhone works today.

Developers buy Macs and install [Xcode](https://developer.apple.com/xcode/) which allows them to write, compile, and deploy iPhone (and iPad, Mac, Watch & TV) apps. To actually experience the user experience of their apps, developers either push the app to their own iPhone and launch it like any other app, or they run it directly on their Mac [within the Simulator](https://developer.apple.com/documentation/xcode/running_your_app_in_the_simulator_or_on_a_device). They choose which device they want to simulate and then see an interactive representation of the device’s screen running their app within a window on their Mac.

![Screenshot showing the run destination menu in the toolbar where you choose a real device, choose a simulated device, or create a custom simulator.](apple-ar-app-developer-story/xcode-choose-simulator.png)

Could you one day choose AR Glasses from the list here?

Currently this has worked by compiling the iPhone or iPad software for Intel chips, allowing the app to be run ‘natively’ on the Mac. Macs are powerful enough to run several of these simulators at once, however checking graphic intensive experiences such as 3D or animation sometimes means avoiding the simulator and trying the app directly on the target device. On the whole the simulator does a capable enough job to preview the experience of an app.

(An iPad version of Xcode has been speculated for years, been even with their improved keyboards and fancy trackpads, nothing has been released. The Mac maintains its role as _the_ developer device for Apple’s platforms.)

How will this work for the AR glasses? Will Xcode provide an AR glasses Simulator for Mac? Would that appear as a window on screen with a preview for each eye? Or would you need to push the app to an actual device to preview?

If a simulator was provided, the pre-Apple-Silicon technology of an Intel chip and AMD GPU would not be able to reproduce the capabilities of a unified memory architecture, tiled rendering, and the neural engine. It would either run poorly, at low frame rates, or some capabilities might not even be possible at all. An Intel Mac can simulate software but it cannot simulate hardware. A Mac with related Apple Silicon hardware would allow a much better simulation experience.

Instead of seeing a preview of the AR display on your Mac’s screen, consider if the product could pair directly to your Mac. The developer could see a live preview of their work. The Mac could act as a host device instead of the iPhone, providing the computation, powerful graphics, machine learning, and networking needs of the AR glasses.

With the [same set of frameworks brought over](https://developer.apple.com/macos/iphone-and-ipad-apps/) allowing iPhone & iPad apps to be installed and run on the Mac, both software and hardware will be ready to run AR-capable apps designed for iPhone. The Mac is now a superset of iPhone, and so what the iPhone can do, the Mac can also do. App makers now have a unified _developer_ architecture.

Perhaps AR-capable apps from the iPhone App Store could even be installed by normal users directly on their Mac. With _augmented_ reality perhaps the glasses will augment the device you currently use, whether that’s the iPhone in your pocket or the Mac on your desk. And allow switching back-and-forth as easily as a pair of AirPods (which would likely be used together with AR glasses).

There’s one last picture I want to leave you with. [Swift Playgrounds](https://www.apple.com/swift/playgrounds/) works by showing a live preview of interactive UI alongside editable code. Change the code and your app immediate updates. The Simulator has been integrated into the app developer experience.

Now imagine Swift Playgrounds for AR — as I edit my code do my connected AR glasses instantly update?
