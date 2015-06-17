# Browser Rendering Optimization
Notes on browser rendering optimization and hitting 60fps smoothness!

## App Life Cycles
 * Response
 * Animations
 * Idle
 * Load (XHR, Websockets, HTML imports)

## In Chronological Order
1. Load (~1 sec) Initial page load
2. Idle (~ 50ms) Lazy load items
3. Response (~100ms) On interaction, respond within 100ms
4. Animations (~16ms) In reality we get ~12ms since the browser has some overhead


## For an opacity change or transform animation, only the composite is triggered.
* The page isn't receiving any new HTML, so the DOM doesn't need to be built.
* The page isn't receiving any new CSS, so the CSSOM doesn't need to be built.
* The page isn't receiving any new HTML or CSS, so the Render Tree doesn't need to be touched.
* If an opacity or transform changes affects element on its own layer, layout won't need to be run.
* If an opacity or transform changes affects element on its own layer, paint won't need to be run.


## JavaScript code to run faster
* Don't concentrate much on micor optimizations eg. for loop vs while etc, sicne different JS engines (V8 etc.) handle it in different ways.
* JS can trigger every part of the rendering pipeline(Style, layout, paint and compositing changes), hence run it as early as possible in each frame.

## Animation
* requestAnimationFrame is the goto tool for creating animation.
* Browser has the render frames at 60fps ie. 16ms/frame.
	* Due to browser overhead, we get around 10ms, so JS has about 3ms to use
* JavaScript -> Style -> Layout -> Painting -> Composite
* Do not use setTimout or setInterval for animations since JS engine does not pay attention to the rendering pipeline when executing this.
* For IE9 - use requestAnimationFrame with Polyfill.

## Webworkers
* Webworkers provide an interface for spawnign scripts top run in the background, in a totally different scope than the main window and also in separate thread.
* Webworkers and a main thread can communicate with each other.

## Styles and Layout (Recalc Styles)
* The cost of recalculate styles scales linearly with the number of elements on the page.
* BEM: Block Element Modifier: Use this style for CSS selectors.
* Class matching if often the fastest selector in modern browsers.
* Reducing 'Recalculate Styles' time.
	* Reduce affected elements (fewer changes to render tree)
	* Reduce selector complexity (fewer tags & class names to select elements)
* 'Forced synchronous layout' occurs when you ask the broswer to run 'layout' first inside the JavaScript section and then 'style' calcs and then layout again. Try to avoid it. Chrome dev tools (flame chart) helps to identify this.
* Read layout properties and batch your style changes to avoid running layout as much as possible.
* Layout thrashing happens when you do a forced synchronous layout many times in succession.

## Compositing and Painting
* Update Layer tree: Happens when Chrome's internal engine (Blink) figures out what layers are needed for the page. It looks at the styles of the elements and figures out what order everything shoudl be in and how many layers it needs.
* Composite Layer: Is where the browser is putting the page together to center the screen.

Thanks to [Paul Lewis](https://twitter.com/aerotwist) and [Cameron Pittman](https://twitter.com/cwpittman) for their course on Udacity, which presented deeper insights to this topic.

























