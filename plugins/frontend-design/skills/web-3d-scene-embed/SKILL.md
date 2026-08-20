---
name: web-3d-scene-embed
description: Puts an interactive 3D scene on a web page, and decides first whether it should be there at all. Covers the qualify-first branch that sends a decorative scene back as a video, the polygon, light and texture budgets that must be written before the scene is authored, the page cost arithmetic including the video memory a texture set occupies regardless of what the files compressed to, deferred loading, exclusion from server rendering, container sizing, the poster and timeout fallback, the mobile path, the control layer, and the four blank-screen signatures with their causes. This skill should be used when a 3D scene is being added to a page, when a scene renders as a blank canvas or breaks a server-rendered build, or when a page carrying a canvas has to hold a performance budget.
---

# Interactive 3D scene embed

## The claim this skill is built on

Nearly every painful 3D integration was attempted in the wrong order. The component went into the page, it worked on the machine it was written on, and then performance, mobile, server rendering and the fallback were handled afterwards as bugs.

Each of those is nearly free before the scene exists and expensive afterwards, because the fix is usually a change to the scene, and by then the scene is finished, the person who authored it has moved on, and the only lever left is the one that costs visual quality. Budgets are a brief. Written first, they are three numbers in a hand-off note. Written last, they are a request to redo the work.

The second claim is that the correct output of this procedure is sometimes no scene. A decorative object drifting behind a headline conveys nothing that a video does not, at roughly a hundred times the cost, and the decision to ship it as 3D is almost never made explicitly. It is inherited from a mood board and then defended after the fact.

The third claim is structural. Build the page so the scene can be deleted in one commit and the page still works. That single constraint forces the static path to exist, forces the layout space to be reserved, forces the runtime out of the entry chunk, and gives you an escape route when the scene turns out to be too expensive on the hardware your users actually have.

## Step 0. Qualify, and be willing to answer no

Run these in order and stop at the first that resolves.

1. **Does anything in the scene change in response to the user or to application state?** If nothing does, it is a video. Render it once, ship the file, and stop here.
2. **Does it change only with time?** An idle loop, a slow drift, a looping animation. Still a video. A short loop plays on every device, decodes on dedicated hardware rather than the main thread, and survives a lost graphics context.
3. **Does it change only along one fixed path?** A turntable is an image sequence: 24 to 36 frames, preloaded, scrubbed on drag. A scroll-driven fly-through is a pre-rendered video whose current time is set from scroll position. Both cost a fraction of a runtime and neither needs one.
4. **Does it respond to arbitrary user input, or to state the page holds?** Rotating to inspect a real object, changing a configuration and seeing the result, reading spatial data with genuine depth. Now 3D is a candidate. Continue to step 1.
5. **Traffic gate.** If most sessions are on phones, or the page currently sits just inside a performance budget it must hold, the interactive scene is the exception rather than the default. Ship the static path as the default and put the scene behind an explicit control.
6. **If you cannot tell**, because there is no device data and no field measurement, do not resolve it by argument. Build the static path first and ship it. Add the scene as an enhancement behind a control, labelled so the user knows what they are asking for, and instrument how many people press it. The order does the deciding for you, and the users who would have been hurt by the wrong answer are exactly the ones who never complain.

**The deletion test, which is how you know step 6 was done honestly.** Remove the scene component from the page and rebuild. If the page still renders, still has the same layout, and still shows the product, the integration is right. If the hero collapses, the layout shifts, or a section is now empty, the scene was not an enhancement and the static path does not really exist.

## Step 1. Choose the integration shape

Three shapes, chosen by what the scene has to talk to, not by what is fashionable.

**A wrapper component in a declarative component framework.** The scene graph is expressed as components and the framework reconciles it. Correct when the page is already built this way and the scene has to read application state, because the binding is then the framework's ordinary data flow rather than a bridge you maintain. Cost: you now have two moving versions, the wrapper and the runtime, and they must match. See step 10.

**The imperative runtime directly.** You create the renderer, own the loop, handle resize and dispose of resources yourself. Correct when the page is not built from components, when the scene is controlled by something outside the component tree, or when you need the smallest possible surface. Cost: everything the wrapper was doing for you is now yours, and the resource disposal in particular is the part people forget.

**An iframe or a hosted embed.** Correct when you need neither state binding nor programmatic control, and by far the cheapest to integrate. Cost: you cannot defer the work happening inside it, you cannot restyle it, and you have taken a dependency on somebody else's release schedule and uptime. The one deferral you do get is `loading="lazy"` on the iframe element, supported in Chromium since 2019 and in current Safari and Firefox, with Firefox the last of the three to add it. Verify support for your own target list rather than assuming it.

## Step 2. Write the budget before the scene is authored

Send these numbers to whoever is building the scene, in the hand-off, before modelling starts. They are ceilings, not targets, and they are approximate.

| Budget | Desktop | Mobile |
|---|---|---|
| Triangles in the whole scene | 150,000 | 50,000 |
| Real-time lights | 3 | 1 |
| Shadow-casting lights | 1 | 0 |
| Largest texture dimension | 2048 | 1024 |
| Complex scenes per page | 1 | 1 |

Four instructions that go in the same note, because they change how the scene is built rather than how it is exported.

- **Bake the lighting** for anything that does not have to respond to something changing. A baked lightmap plus a small environment map costs almost nothing per frame, and each shadow-casting light adds a whole extra pass over the scene.
- **Prefer cheap shading.** Materials that fake their shading are dramatically cheaper than physically-based ones lit for real, and at the size most web scenes render, the difference is visible only side by side.
- **Delete what is not visible.** Hidden objects, off-camera geometry, the interior of a sealed object and the modelling scene the object was built in all ship if nobody removes them.
- **Turn on geometry and image compression at export**, and quantise position and normal precision. Name the geometry options rather than the category, because they are not interchangeable: `KHR_mesh_quantization` stores positions, normals and texture coordinates at reduced precision and needs no decoder at all, `EXT_meshopt_compression` layers a small and fast decoder on top of that, and `KHR_draco_mesh_compression` usually produces the smallest file of the three and charges you a larger decoder and more decode time for it. Which one suits your particular asset is out of scope here. The extensions are specified in the Khronos glTF extension registry, and the glTF Transform documentation covers applying them and reporting what a file actually contains. The image half of this bullet is step 3's subject. The default of doing none of it is the one that is definitely wrong.

**Derive the texture resolution from the rendered size rather than from habit.** Measure how wide the object actually appears. A canvas 480 CSS pixels wide at a device pixel ratio of 2 is 960 device pixels, so a 1024 texture on the largest object in it is already at parity and a 4096 is four times more than can ever be seen.

## Step 3. Do the page arithmetic, including the memory nobody counts

Total the page cost in writing, before the asset is built, as a table.

| Item | Download | Memory |
|---|---|---|
| Runtime plus loaders and controls | around 500KB compressed, measure your own build | small |
| Decoder or transcoder, if used | tens to a few hundred KB | small |
| Scene file | commonly 1 to 10MB | geometry roughly in proportion |
| Textures | whatever the files compressed to | **width times height times four bytes** |

That last cell is the arithmetic that decides whether the page survives a phone, and it is almost never done.

**An uncompressed texture occupies width times height times four bytes in video memory regardless of what the file compressed to on disk.** A PNG is compressed on disk and completely uncompressed once uploaded to the graphics processor. A full mip chain adds about a third.

- 1024 by 1024: about 4.2MB, about 5.6MB with mips.
- 2048 by 2048: about 16.8MB, about 22.4MB with mips.
- 4096 by 4096: about 67MB, about 89MB with mips.

So four 4096 textures are roughly 356MB of video memory from files that may have downloaded as 12MB. The download looks fine in the network panel and the tab dies on a phone.

**Compressed texture formats are the fix, and they are the only one.** A PNG or a JPEG is compressed on disk and raw in video memory. A block-compressed texture stays compressed in video memory, because the graphics hardware samples the compressed blocks directly.

**What to ship: KTX2 files carrying Basis Universal supercompression**, referenced from the scene through the `KHR_texture_basisu` glTF extension. Basis has two modes and the choice is not cosmetic. ETC1S is the small one and belongs on colour and albedo maps. UASTC is the large one and belongs on normal maps and anywhere a compression artefact reads as an error rather than as softness.

**What it costs.** A transcoder, which is a WebAssembly module fetched alongside the loader and is the decoder row in the table above rather than a rounding error. Decode time at load, once per texture, because a KTX2 file is transcoded on arrival into whichever compressed format the device supports: a BC format on desktop, ASTC on most current mobile hardware, ETC2 on older Android. And the part that looks like a regression: **a KTX2 texture is frequently larger on disk than the JPEG it replaces.** You are trading download bytes you can see for video memory you cannot, at roughly a quarter to an eighth of the uncompressed figure depending on the block size, so a 2048 by 2048 map lands nearer 4MB than 16.8MB before mips. That trade is the whole point, and the network panel is the wrong place to judge it.

**The one case to check rather than assume.** If a device supports none of the target formats, the only remaining output is uncompressed RGBA, and the memory figure returns to the arithmetic above. Verify that on the oldest phone in your traffic, not on your laptop.

**The totals to hold yourself to.** A typical well-made scene adds 1 to 5MB to the page. An unoptimised export adds 20MB or more. If your table totals over 5MB of download, or over about 100MB of texture memory, go back to step 2 rather than forward to step 4.

## Step 4. The load path

**Nothing about the scene may run before first paint.** Not the runtime, not the loader, not the decoder.

- Import the runtime and the component **dynamically**, at the point of use. A lazy component wrapped in a suspense boundary, or a plain dynamic import inside an event handler or an effect.
- For anything below the fold, gate the dynamic import behind an **intersection observer** with a root margin of a few hundred pixels, so loading begins slightly before the canvas arrives and not on page load.
- For anything above the fold that survived step 0, load after the page is interactive or on an explicit control press, never during the initial render.

**The trap that defeats all of the above** is a static import somewhere else in the module graph. If any module that the entry chunk reaches imports the runtime at the top level, for a type, for a constant, for a helper, the bundler pulls the whole runtime into the entry chunk and the lazy component is lazy for nothing. Check the built output, not the source: the runtime must not appear in the initial chunk. A type-only import is safe when it is written as one and erased at build time. A convenience re-export from a shared index file is the usual culprit.

## Step 5. Exclude the component from server rendering

Graphics runtimes touch the window object, and often at module scope, so they cannot be evaluated on a server. The symptom is a build or a first request failing with a message that the window, the document or the self object is not defined.

The fix depends on the rendering model rather than on the product.

- **Client-rendered application, no server pass.** Nothing to do.
- **A framework that server-renders components before hydration, older style, where every page is a client component with a server pass.** Import the scene component dynamically with server rendering disabled. This is a single option on the dynamic import.
- **A server-components model, of the kind React-based meta-frameworks adopted from 2023 onwards.** The same option is not permitted inside a server component and the build will tell you so. The scene component must be marked client-side, and the dynamic import with server rendering disabled has to live inside a component that is already client-side. Wrapping it one level down is the usual answer. Verify against your framework's current version, because this specific rule has changed more than once.
- **An islands or partial-hydration model.** Mark the island client-only and give it a visible or idle load directive rather than an eager one.

Whatever the model, the placeholder that renders on the server is the poster image from step 7, so the server output is never an empty box.

## Step 6. Give the container an explicit size

A canvas sized in CSS to fill its parent will render nothing at all if the parent's height resolves to zero, and it will throw nothing while doing it.

- Give the wrapper an **explicit height**, or an aspect ratio plus a width, or a height derived from the viewport. A percentage height only works when every ancestor up the chain has a resolved height, which in practice it does not.
- **Reserve that space before the scene loads**, in the same markup that renders on the server. This is what stops the layout shifting when the canvas appears, and it is the same box the poster sits in.
- On resize, update both the renderer size and the camera aspect ratio. Updating one without the other stretches the scene, which looks like a modelling error and is not.
- Clamp the device pixel ratio to at most 2. A phone at a ratio of 3 is rendering nine times the pixels of a ratio of 1, and clamping is usually worth more than any geometry change.

## Step 7. The placeholder, and the timeout nobody adds

Show the poster image immediately, in the reserved box, and cross-fade to the canvas on the runtime's load callback. Without this the user looks at empty space for two to five seconds and concludes the page is broken.

**Then add the timeout.** If the load callback has not fired within a fixed budget, five seconds is a reasonable default, stop waiting, keep the poster, and log it. Every failure in the next section other than the zero-height parent ends with a load callback that never fires, and without a timeout the user sits in front of a poster forever with nothing on screen and nothing in the console. A scene that quietly degrades to its own poster is a good day. A scene that hangs is a support ticket.

The same box also carries a real text alternative and a link or control that reaches the same information without the scene. What makes one adequate depends on which kind of scene it is, and the two kinds have different obligations.

**A decorative scene owes nothing but an exit.** If it carries no information the surrounding page does not already carry, the correct alternative is no alternative: hide the container from assistive technology so it is skipped entirely, and do not describe the shape. A paragraph narrating a rotating abstract object is noise inserted into a page somebody is trying to read, and WCAG 1.1.1 says as much for anything that is pure decoration.

**An informative scene owes the information, not a description of the widget.** The test is not whether you described what is on screen, it is whether a person who never sees the canvas ends up knowing the same thing. "An interactive 3D model of the product" describes the control and conveys nothing. What a viewer would learn by rotating it does: the proportions, the ports on the back, the fact that the hinge folds flat. Write that as text, in the page, near the scene rather than hidden behind it.

**A configurator owes its state and its options too.** The current selection has to be readable as text rather than existing only as a colour on a mesh, and every option the scene offers has to be reachable by a control outside the scene. Where that is missing it is not only an accessibility defect, it is a feature with exactly one route into it.

Whether what you wrote actually passes is a separate procedure, and the [WCAG 2.2 audit](/skills/wcag-audit/) is where that lives.

## Step 8. Ship a mobile path

Decide this at step 2, not at the end. Two acceptable answers.

- **A reduced scene**: fewer triangles, one light, no shadow pass, half-resolution textures, selected at load from a breakpoint or a device query. This means the asset pipeline produces two files, which is a cost the person authoring the scene needs to know about in advance.
- **A static swap**: below a breakpoint, typically 768 pixels, the poster is the final state and the runtime is never fetched. Note that this is a decision about the fetch, not about display. Hiding a canvas with CSS after loading it has paid the entire cost for nothing.

Also honour a reduced-motion preference: it must stop autoplay orbits and idle drift while leaving manual interaction working.

## Step 9. Add the control layer last

Only once loading, sizing and fallback are right. Bindings written before that get debugged against a scene that is not appearing for unrelated reasons.

- **Find objects by name**, and treat those names as a contract with the scene author. Write down the list the code depends on, check every name at load, and log a clear warning for a missing one rather than throwing. A rename in the modelling tool otherwise breaks the page silently and the console says nothing.
- **Listen for events the scene already defines** rather than adding hit testing of your own.
- **Bind application state to scene variables**, so the scene reads a value and decides what to do with it. This survives the scene being re-authored; direct mutation of a named object's transform does not.
- **For scroll-linked scenes, use one passive scroll listener that writes a single normalised progress value between 0 and 1**, and read that value inside the render loop. Do not mutate the scene inside the scroll handler. One listener plus one variable is the pattern that stays at frame rate under fast scrolling, because the work happens once per frame instead of once per event.
- **Stop the loop when the canvas is off screen or the tab is hidden.** One intersection observer and one visibility listener.

## Step 10. Versions and delivery

**Pin the wrapper and the runtime to versions known to work together, and install them in one command.** A wrapper installed months after the runtime resolves to whatever is current, and the pair drifts. Two copies of the runtime in the dependency tree is the worse case: objects created by one are not recognised by the other, type checks fail silently, and the screen is blank with no error. Check for duplicates in the resolved tree, and use the lockfile's override or resolution mechanism to force a single copy.

**Serve the asset from your own origin.** A hosted preview URL from a design tool is not a delivery path: it can be rate limited, it can change, and it puts a cross-origin header requirement between your page and your product. If a cross-origin error appears, copy the file into your own static assets rather than negotiating with headers.

## The verification gate

Do not call it done until all six pass. The first four are numbers, the last two are a diff.

1. **Interactive within 3 seconds on desktop**, from a cold cache on a normal connection.
2. **Interactive within 5 seconds on a mid-range phone**, or the timeout fires and the poster stays.
3. **60fps on desktop**, and a floor of 30fps on the phone, measured after two minutes rather than two seconds.
4. **A measured delta on the page's own metrics.** Build the page with the component removed, record the largest contentful paint and the blocking time, restore it, record again. A prediction is not a measurement, and the delta is the only honest number.
5. **The runtime is absent from the entry chunk** in the built output.
6. **The deletion test passes.**

## Failure modes, named, and what each looks like from the outside

**Blank canvas, clean console, successful download.** The canvas element has a real width and a height of zero, because its parent's height never resolved. Nothing throws because nothing is wrong from the runtime's point of view: it drew your scene into a box of no height. Recognisable by inspecting the element and reading the two numbers.

**Server rendering crash on the window object.** The build fails, or the first request returns an error naming window, document or self as undefined. The component was evaluated during a server pass. Distinguishable from the others because it happens before anything reaches a browser.

**Version skew, or two copies of the runtime.** Blank screen, no error, network requests all successful, and often a console warning about multiple instances being imported that everybody ignores because the page mostly works. The tell is in the dependency tree rather than in the code: list the installed versions of the runtime and count them.

**The runtime loaded eagerly, wrecking largest contentful paint.** Everything renders correctly and the page is simply slower, so nobody attributes it to the scene. The tell is in the build output and the network waterfall: the runtime is in the initial chunk, or it is requested before the hero image. Almost always caused by one static import in a shared module rather than by the component itself.

**No mobile path.** The phone downloads the desktop scene, spends four seconds decoding it, runs at eight frames per second and gets hot. Nobody on the team sees it because nobody on the team browses the site on a three-year-old phone.

**Decorative 3D that should have been a video.** The scene works, meets its budgets, and is still the wrong answer. Recognisable by the fact that nobody can name a user action that changes anything in it.

**The scene created twice in development.** A development mode that deliberately double-invokes effects will create two renderers and two canvases if the setup has no cleanup path. The tell is two canvas elements in the inspector, doubled memory, and a frame rate that is fine in the production build. The fix is a real teardown in the cleanup, which is the same code that stops the route-change leak in production.

**The poster that never leaves.** The load callback never fires, because the event name was wrong, the asset 404s, or the graphics context was refused. With no timeout, the poster sits there and the page looks finished but dead. Recognisable because it is indistinguishable from success on a fast machine.

## Worked example, compressed

A marketing site for a project management tool. The hand-off shows an abstract translucent shape rotating slowly behind the headline. The page currently reports a largest contentful paint of 2.1 seconds.

**Step 0.** Does anything in it change in response to the user? No. To application state? No. Only with time? Yes, it rotates. That resolves at step 2: it is a video.

**The arithmetic, written down so the decision can be argued with.** As proposed the scene would carry a runtime with loaders and controls at roughly 500KB compressed, a 1.8MB scene file, and two 2048 textures plus one 1024. Texture memory is 16.8 plus 16.8 plus 4.2, which is 37.8MB, or about 50MB with mip chains. Total download around 2.4MB before the textures are counted properly, plus a persistent 50MB of video memory, plus a render loop running for as long as the tab is open, plus a graphics context that can be lost. For an object that no user will ever touch.

**The alternative, priced.** A 6 second silent loop at 1280 by 720, encoded for the web, lands in the region of 700KB, decodes on dedicated hardware, costs nothing on the main thread, works with no graphics context, and can be paused for a reduced-motion preference with one attribute. It is also the same asset the poster would have been.

**Verdict: no scene.** Ship the video loop with a still first frame as the poster and the page keeps its 2.1 second paint. The 3D adds nothing a user can act on and would have cost roughly ten times the bytes and fifty megabytes of video memory to say the same nothing.

**The other half of the same hand-off passes.** Further down the page, a configurator lets the visitor pick a colour and rotate to see the back. That is arbitrary user input against application state, so step 4 resolves and the build spec goes back to the scene author: 50,000 triangles, one baked lighting setup with a single real-time light, 1024 textures, no shadow pass, geometry compression on at export, and a second reduced file for phones. It loads on an intersection observer, in a box with a fixed aspect ratio, behind a poster with a five second timeout, and the whole component can be deleted in one commit leaving a product photograph in its place.

## What this skill does not do

- It does not author or optimise the scene. It writes the budget and builds the integration, and the modelling, merging and texture conversion belong to a 3D asset pipeline.
- It cannot measure. Every figure here is arithmetic over an asset list, and the verification gate at the end is a thing you run, not a thing this produces.
- It does not judge an integration that already exists. Draw call arithmetic is out of scope, and so is choosing between the geometry compression extensions for one specific asset: the first is a profiler question and the second is settled by running a glTF pipeline over the file and comparing the outputs. Failures that only surface after a long run, thermal throttling and memory that grows over an hour, are out of scope as well, because they need a device and a stopwatch rather than a procedure.
- It is not an accessibility audit. It requires a text alternative and a non-3D route to the same information, and it does not test whether either is any good.
- It has no view on whether the scene looks right. Art direction, materials and lighting quality are outside it, and a scene can meet every number here and still look cheap.
- It cannot tell you whether the interactive version sells more than the static one. That is a test with real traffic, and it is frequently the only question that matters.
