---
name: web-3d-integration-review
description: Reviews an interactive 3D scene against the web page it is going into, using real budgets rather than impressions. Covers polygon and draw call ceilings for desktop and mobile, the difference between texture file size and GPU memory, compressed texture formats and mipmapping, glTF delivery with Draco or Meshopt, the cost to Core Web Vitals, the named ways a scene fails outside the studio, the accessible fallback a canvas always needs, and the cases where a video or a static render is the correct answer instead. This skill should be used when a 3D scene is proposed for a page, before a scene file is committed, or when a page containing a canvas performs badly on mobile.
---

# Interactive 3D on the web, review and budget

## The claim this skill is built on

Almost every failed 3D integration on the web failed at arithmetic that nobody did.

The scene ran at sixty frames per second on the machine it was built on, which had a discrete graphics card, a fast connection, and no other tabs open. Nobody computed how much video memory the textures would occupy, which is a different number from the file size and is usually ten to twenty times larger. Nobody counted draw calls. Nobody asked what happens on a phone that is an order of magnitude or more weaker at the same job, running at three device pixels per CSS pixel, and thermally throttled after two minutes.

The second claim is that the largest decision is whether the 3D should exist at all, and that decision is almost never made explicitly. It is inherited from a mood board.

This skill does the arithmetic, names the failures that only appear outside the studio, and puts the "do not use this" case in writing so it can be argued with.

## The budgets

These are heuristics for marketing pages, product pages and configurators running on commodity hardware in 2026. They are not laws, and a game or a serious visualisation works to entirely different numbers.

**Geometry.** Keep a scene under roughly 150,000 triangles for desktop and roughly 50,000 for mobile. These are approximate and they are ceilings rather than targets. A single well-made product model is usually good at 20,000 to 60,000 triangles once it is normal-mapped, and the extra detail beyond that is very often invisible at the size it renders on screen.

**Draw calls.** More important than triangles, and covered in its own section below. As a rough ceiling, low thousands on desktop and a few hundred on mobile, approximate.

**Lights.** Three real-time lights maximum on desktop, one on mobile. Each real-time light multiplies the shading work for every lit fragment, and each shadow-casting light adds an entire extra render pass over the scene. A scene with five shadow-casting lights is rendering itself six times. Prefer baked lighting and image-based lighting from a small environment map, which cost almost nothing per frame.

**Bytes on the wire.** A general purpose 3D runtime costs on the order of a few hundred kilobytes of JavaScript after compression, before loaders, controls or post-processing, and each decoder you add is a further download. Scene files commonly land between 1 and 10 megabytes, and textures push that further, frequently past everything else combined. Approximate, and worth measuring on your own build rather than trusting.

**The mobile gap.** Treat a mid-range phone as an order of magnitude or more weaker than a desktop machine at fragment work, with a fraction of the memory bandwidth and a hard thermal ceiling that desktop hardware does not have. This is why "it runs fine on my machine" carries no information about whether it ships.

## What it costs the page

**The threshold that decides this.** Largest Contentful Paint is good at 2.5 seconds or less, assessed at the 75th percentile of real user traffic, and it is the metric a hero scene threatens. Total Blocking Time is a laboratory metric rather than a Core Web Vital, and it is the one that a decoder running on the main thread moves most. The full Core Web Vitals table, the other two thresholds and the dates they changed, belong to the Perceived performance audit skill.

**The specific mechanism.** A canvas element is not itself a Largest Contentful Paint candidate under the definition in force at the time of writing, so the risk is not that the scene is measured as your LCP element. Confirm that before relying on it, because the candidate list has been extended more than once. The real cost is two-sided. First, the scene competes for the same network and the same main thread as whatever your LCP element actually is, so it delays it. Second, if a poster image is placed behind the canvas while the scene loads, that image very much can be the LCP element, and it will be measured.

A hero 3D scene will typically add seconds to LCP and hundreds of milliseconds of blocking time if it loads eagerly. Those figures are approximate and depend entirely on the asset, which is the point: they are large enough that they must be budgeted rather than absorbed.

**Two rules that follow.** The scene must never load before first paint, and it must never be the thing the page is waiting on to become useful. Load the runtime and the asset after the page is interactive, or behind an explicit user action, and reserve the layout space up front so nothing shifts when it arrives.

## Texture discipline

This is where the arithmetic is, and where the biggest wins are.

**File size is not GPU memory, and the gap is enormous.** A PNG is compressed on disk and completely uncompressed once uploaded to the graphics processor. An uncompressed texture occupies **width times height times four bytes** in memory, whatever the file compressed to.

- 1024 by 1024: about 4.2 megabytes, about 5.6 with a full mip chain.
- 2048 by 2048: about 16.8 megabytes, about 22.4 with mips.
- 4096 by 4096: about 67 megabytes, about 89 with mips.

A full mip chain adds about a third. Six 4096 textures for one model is therefore over half a gigabyte of video memory, from files that may have downloaded as 20 megabytes of PNG. That is the arithmetic nobody does, and it is why the tab crashes on a phone.

**Compressed texture formats fix it, because they stay compressed in memory.** KTX2 with Basis Universal is the practical answer: one file that is transcoded at load time into whichever block-compressed format the device supports. At roughly one byte per texel for a common block format, the same 2048 texture occupies around 4.2 megabytes rather than 16.8, and about 5.6 with mips. That is a four-fold reduction that persists for the life of the page, unlike a download saving which is spent once. The cost is a transcoder to download, on the order of a couple of hundred kilobytes, and some decode time.

**Dimensions.** Powers of two are the safe default. WebGL 1 requires them for mipmapping and for repeat wrapping; WebGL 2 and WebGPU do not, but block compression formats operate on blocks of four texels by four, so dimensions should be multiples of four at minimum and powers of two remain the least surprising choice.

**Mipmapping is not optional.** Without mips, a texture sampled at a smaller size than its resolution aliases and shimmers during motion, and it also wastes memory bandwidth, because the hardware reads texels it will discard. Mips cost a third more memory and pay for themselves immediately.

**Resolution is a budget, not a quality setting.** Ask what size the object actually occupies on screen. A component that renders at 400 pixels wide does not benefit from a 4096 texture in any way a user can see.

## Geometry, draw calls and lighting

**Draw calls matter more than triangles.** Each draw call carries fixed processor overhead to set state and issue the command, and that overhead does not care how many triangles the call contains. A scene of forty objects at 3,000 triangles each is normally slower than a single object at 120,000, despite identical geometry.

The consequences invert the usual instinct:

- **Merge meshes** that share a material into one geometry. This is usually the single largest win available and it costs no visual quality at all.
- **Use instancing** for anything repeated. A hundred identical chairs should be one draw call with a hundred transforms, not a hundred draws.
- **Reduce material count**, since each distinct material is at least one more draw call, and texture atlasing lets several objects share one.
- **Decimate last.** Reducing triangle count is the obvious move and usually the least effective one, and it is the one that costs visual quality.

**Baked lighting beats real-time lighting** for anything static. Lighting computed offline into a lightmap or into vertex colours costs nothing per frame, looks better than a few real-time lights normally manage, and removes shadow passes entirely. Reserve real-time lights for what genuinely has to respond to something changing.

## Model format and delivery

**glTF 2.0 is the delivery format.** Released by Khronos in June 2017 and published as an international standard, ISO/IEC 12113:2022, it is designed for transmission and runtime loading rather than for authoring. Use the binary container, which packs geometry, textures and the scene description into one file.

**The source format should never ship.** Authoring formats from modelling tools carry construction history, modifiers, unapplied transforms, the full-resolution textures, and frequently the entire scene the model was built in. They are also slower to parse and are not guaranteed to load the same way twice. Export to glTF, then process the glTF.

**Geometry compression, and its cost.** Draco compresses glTF geometry substantially, commonly to a fraction of its uncompressed size, at the price of a WebAssembly decoder to download, on the order of a couple of hundred kilobytes, and decode time that is noticeable on a mid-range phone and can run into hundreds of milliseconds for a large model. That decode is main-thread work unless you move it to a worker, and it lands squarely in blocking time.

Meshopt is the alternative worth knowing: a much smaller decoder, on the order of tens of kilobytes, and faster decode, in exchange for less aggressive compression. Both figures are approximate. **The rule is that geometry compression is a trade of download bytes for processor time, and on mobile the processor time is frequently the scarcer resource.** For a small model, no compression at all is often the fastest end to end.

**Quantise attributes** before reaching for a compressor. Positions and normals stored at reduced precision shrink the file with no decoder cost whatsoever, and for many models that is enough on its own.

## Failure modes, named, and what each looks like from the outside

**The blank canvas with no error.** The canvas is sized in CSS to fill its container, and the container's height resolves to zero because nothing gives it one. The canvas is therefore zero pixels tall, renders nothing, and throws nothing. The console is clean, the network tab shows the scene downloaded successfully, and the page looks broken. Recognisable because inspecting the canvas shows a real width and a height of zero.

**The cross-origin failure.** The scene or its textures are served from a different origin without the right headers, so either the fetch is blocked outright or, more confusingly, the texture loads but taints the canvas, and any later attempt to read pixels from it throws a security error. The symptom is a scene that renders until the moment something tries to screenshot it or read it back.

**The version-locked pair.** The runtime and one of its loaders come from different versions, or from different module resolutions of the same version. Either it throws at import, or, worse, two copies of the library end up loaded, so objects created by one are not recognised by the other and type checks silently fail. The tell is a console warning about multiple instances of the runtime being imported, which is frequently ignored because the page still appears to work.

**Sixty on the workstation, eight on the phone.** The scene was tuned on hardware with a discrete graphics card. The phone is fill-rate limited, is rendering at a device pixel ratio of three unless told otherwise, which is nine times the pixels of a ratio of one, and has a fraction of the memory bandwidth. The usual first fix is clamping the pixel ratio to at most two, which is often worth several times the gain of any geometry change.

**Thermal throttling on a long-lived scene.** Everything is fine for the first minute or two and then the frame rate falls and stays down, with nothing in the code having changed. Sustained graphics load heats the device, the system reduces clocks to protect it, and battery drains fast enough for the user to notice. Recognisable because performance degrades with time rather than with what is on screen.

**The loop that never stops.** The animation loop keeps running when the tab is hidden or when the canvas has scrolled far off screen. Browsers do throttle both mechanisms in a background tab, but they throttle them to different floors and neither goes to zero: frame callbacks stop being served, while timers are clamped to roughly one call a second, tightening to about one a minute after several minutes hidden in current Chrome, and to a similar clamp in Firefox. So a timer-driven loop keeps waking the device all afternoon at a rate low enough that nobody notices it in a profile. And no amount of background throttling covers the case that matters more, which is a **visible** tab where the canvas has scrolled nowhere near the viewport and every frame is being rendered at full rate for nobody. The symptom is a laptop fan that runs while the user reads the footer. Use an intersection observer to stop the loop when the canvas leaves the viewport, and the page visibility event to stop it when the tab is hidden, rather than relying on the browser to do it for you.

**Graphics context loss.** The context is lost when the graphics process restarts or when the system reclaims memory, which happens far more on mobile than developers expect. Without a listener that prevents the default and rebuilds, the canvas goes permanently blank and only a reload fixes it. From the outside it looks like a random, unreproducible failure.

**The route-change leak.** Geometries, materials and textures hold graphics resources that ordinary garbage collection does not release; they must be disposed explicitly. Navigating between routes several times climbs steadily in memory until the tab is killed. The tell is that the crash correlates with the number of navigations rather than with anything on any one page.

**The scene that ignores reduced motion.** A continuous auto-rotating orbit is exactly the kind of large-field motion that a reduced-motion preference exists to stop, and it is the default in most 3D starter code.

## Accessibility and the fallback

**A canvas is invisible to assistive technology.** Whatever is rendered inside it does not exist in the accessibility tree, so a screen reader user gets nothing at all unless you supply it.

Three obligations:

- **A real text alternative.** Not "3D scene". Describe what the scene conveys, at the length the information warrants: what the object is, its materials and colour, and any state the viewer is meant to read from it. If the scene is a product configurator, the current configuration must be available as text, and it must update as the configuration changes, which means a live region rather than a static label.
- **A non-3D path to the same information.** If the only way to see the back of the product is to rotate it, then a keyboard user, a screen reader user and anyone on a device that cannot run the scene cannot see the back of the product. Photographs of the other sides, or preset view buttons that are real buttons, solve it.
- **Reduced motion, honoured properly.** A reduced-motion preference must stop autoplay orbits, continuous camera drift, and any looping animation. It should not disable the scene, because a user who wants to rotate the model themselves is not asking for that.

**The static image path is not a nicety.** Devices without a working graphics context, users on very constrained connections, and anyone whose context was lost all need a path that shows the product. A well-made static render, sized and reserved in the layout before the scene loads, is the same asset that prevents layout shift and gives you a poster image. Build it first and treat the interactive scene as the enhancement.

## The decision procedure

Run these in order and stop at the first that resolves.

**1. Does the user need to change the viewpoint to get the information?** A configurator where choices must be seen, a product where the back matters, spatial data with genuine depth: yes, 3D is a candidate. Decoration that happens to be rendered in 3D: no. Go to the alternatives section below.

**2. Is it blocking the primary task, or sitting above the fold?** If yes, it is not allowed to load before first paint, and it is not allowed to be what the user is waiting for. Reserve its space, show the static render, and load the scene after the page is interactive or on an explicit action. If it is below the fold, load it when it approaches the viewport and unload it when it leaves.

**3. Can you test on a real mid-range phone, throttled?** If yes, the budgets above are a starting hypothesis and the device settles it. Measure frame time after two minutes, not after two seconds, because thermal behaviour is the part that arithmetic cannot reach.

**4. If you cannot tell**, because there is no test device, no field data, and no way to know what hardware your traffic is on, then **default to the static path and put the interactive scene behind an explicit control**, labelled so the user knows what they are asking for. Then instrument it: log how many people press the control and what their frame rate is. This is the honest branch, because without measurement any claim that the budget is met is a guess, and the cost of guessing wrong is borne entirely by the users on the weakest devices, who are also the least likely to complain and the most likely to leave.

## Worked example, compressed

A furniture retailer's product page. The design calls for an interactive 3D chair filling the hero, replacing the current photograph.

**The asset as delivered.** An export from the modelling tool: 480,000 triangles across 34 separate meshes, 11 materials, six 4096 by 4096 PNG textures totalling 38 megabytes on disk, and five real-time lights, three of them casting shadows.

**The arithmetic.** Six 4096 textures with mips is roughly 530 megabytes of video memory. That alone ends the discussion for mobile, before any triangle is drawn. 34 meshes with 11 materials is at least 34 draw calls, multiplied by four passes for the three shadow-casting lights. 480,000 triangles is over three times the desktop ceiling and nearly ten times the mobile one.

**The reduction.** Decimate to 90,000 triangles, which is invisible at the rendered size once normal maps carry the detail. Merge to four meshes by consolidating materials and atlasing, taking draw calls from 34 to 4. Convert to three 2048 KTX2 textures, which is about 16.8 megabytes of video memory with mips rather than 530, and roughly 2.4 megabytes of download rather than 38. Bake the lighting to a lightmap and keep one real-time light plus a small environment map, removing every shadow pass. Total payload with the runtime, the loaders and the transcoder lands near 3 megabytes, approximate.

**The placement decision.** Step 1 passes: a customer genuinely does want to see the back and the underside of a chair, and the fabric options need to be seen. Step 2 fails for the hero: this is the largest thing on the highest-traffic page and it would delay the element the page is actually measured on. So the hero keeps the static render, which is also the poster and the reserved layout space, and a "View in 3D" control loads the scene on demand.

**Accessibility.** The static render carries a real alternative description. Four preset view buttons give the same information without a pointer. The selected fabric and colour are written into a polite live region as they change. A reduced-motion preference stops the idle orbit and leaves manual rotation working.

**Verdict: the 3D is justified, and the hero placement is not.** Ship the static render in the hero with the interactive scene behind an explicit control, on the reduced asset. The scene as originally delivered would not have run on a mid-range phone at all, and the single decision that changes that is the texture conversion, which is worth more than every geometry change combined.

## When not to use 3D at all

Stated plainly, because the answer is often this one.

- **The camera path is fixed.** If the user never changes the viewpoint, you are shipping a graphics runtime to play back an animation. A video file does that better, at a fraction of the bytes, with hardware decoding, on every device, and with the ability to be paused.
- **It rotates and nothing else.** A turntable of a product is an image sequence. Twenty-four to thirty-six frames, preloaded, scrubbed on drag, is a few hundred kilobytes, works everywhere, and needs no runtime at all.
- **It is decorative.** An abstract shape drifting behind the headline conveys nothing. A static render or an illustration conveys the same nothing at a thousandth of the cost, and it is the version that will still be there when the context is lost.
- **The audience is mostly mobile.** If the analytics say most sessions are on phones, the interactive path is the exception rather than the default, and the effort belongs in the static path.
- **Nobody owns it.** A 3D scene has an asset pipeline, a runtime dependency with a version to track, and a failure surface that ordinary front-end monitoring does not cover. If no one is going to maintain it, it will break quietly and stay broken.

## What this skill does not do

- It cannot profile anything. Every number here is computed from the asset and the code, and a frame time on a real device is a measurement this cannot make.
- It has no view on how the scene looks. Art direction, material authoring and lighting quality are outside it entirely, and a scene can meet every budget here and still look poor.
- The budgets are heuristics for typical marketing and product pages. A game, a large data visualisation, or a scene aimed at workstations works to different numbers and this will be wrong about them.
- It does not know your traffic. Every threshold moves with the hardware your users actually have, and only your own field data can settle that.
- It does not do the optimisation. A glTF processing toolkit will merge, compress and quantise far more reliably than a review, and can do it on every build.
- It cannot tell you whether the interactive version sells more than the static one, which is frequently the only question that matters and is answered by a test rather than an audit.
