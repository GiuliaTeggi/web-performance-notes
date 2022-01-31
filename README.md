# web performance notes

Notes from the course https://www.linkedin.com/learning/developing-for-web-performance

- extreme disparity in internet access and service quality
- large variance in devices and software used to access the web
- poor performing sites gets low ranking on Google

➡️ websites and apps need to be fast and efficient for all users no matter what conditions they are working under

## 4 main areas for optimisation: 
- **reducing overall load time**: compressing and minifying all files, reducing the number of files and http requests sent back between the server and the user agent, employing advanced loading and caching techniques and conditionally serving the user with only what they need when they actually need it.
- **making the site  usable as soon as possible**: loading critical components first, to give the user initial content and functionality and then deferring less important features for later using lazy loading to request and display content only when the user gets to or interacts with it + pre-loading features that the user is likely to interact with 
- **smoothness and interactivity**: improving perceived performance through skeleton interfaces, visual loaders and clear indication that something is happening/things are working
- **performance measurements**: using tools and metrics to monitor performance and validate improvements

*Not every performance optimization will fit every situation and needs*

## How to measure performance?
Best practice is to use several different tools to collect data to compare and analyze. 
- **Browser tools**: [Lighthouse](https://developers.google.com/web/tools/lighthouse/) (Chrome), network monitor and performance monitor in modern browsers
- **Hosted third-party tools**: [PageSpeed Insights](https://pagespeed.web.dev/) (Google), [WebPage Test](https://www.webpagetest.org/)

Standard performance measurements: 

- **Time to first byte (TTFB)**: the time it takes for the browser to receive the first byte of the resource it's looking for
- **First Paint**: when the first pixel renders on a screen e.g. a background color
- **First Contentful Paint (FCP)**: when the browser renders the first bit of content from the DOM e.g. images or text
- **Largest Contentful Paint (LCP)**: the time it takes to show the largest content on the screen e.g. the largest image or text block above the fold
- **Time to Interactive (TTI)**: the time it takes for a page to be fully interactive, ready for user input

## Http2 

Files and data sent between browser and server via http ( Hypertext Transfer Protocol). Two versions of http: 
- **(Old) http/1.1** : all files requested by browser are downloaded synchronously, one at a time and in the same order. To get around this browsers usually open 6 parallel connections, but, in addition to time + resources needed to set and manage multiple connections, first file still holds back the rest of the files from downloading ([head-of-line blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking)): if the first response takes a lot of time, other responses have to wait in line.
- **http2**: [multiplexing](https://en.wikipedia.org/wiki/Multiplexing#Digital_broadcasting), that is browser can download files at the same time over the same connection and each download is independent of the others. Responses are received in an arbitrary order so no need to wait for a slow response that's blocking others  ➡️ instant performance improvement
    - **Requirements for http2**: both server and browser must support http2 + the connection must be encrypted over https

## Performance Bottlenecks

- **server/hosting**: processor speed, available RAM, storage type, available bandwidth, shared resources etc.
  - **optimizations**: get sufficiently powerful hosting, explore dynamic cloud options, optimize files for the server (e.g. compressing files and reducing number of http requests), use a CDN where files are cahced on multiple servers. 
- **connection** beetween server and browser: DNS lookup, TCP handshake. 
  - **optimizations**: DNS prefetch and preconnect to servers, preload content, consider server push (to push files to browser before request is made), pre-caching select assets.
- **sending files**, how many and which files are requested: lage css/js bundles, large image files
    - **optimizations**: modularize js and css, take advantage of http2 multiplexing, async/defer js, defer non-critical css, optimize images to reduce file size and lazy-load images, compress all files using gzip and/or Brotli

## Caching

- **On the server**: esp vital for server-side rendered content (e.g. from a CMS), so server doesn't have to generate the same assets for every request. Subsequent requests are given a stored cached snapshot of the page instead of freshly rendered page. Whenever an asset changes page needs to be re-rendered, but caching still extremely worthwile.
- **On the CDN** ( = external caching for assets): CDN renders page when requested and caches it in an edge location close to the visitor.
- **In the browser**: all browsers cache files to some extent automatically (e.g. css and image files), how to handle caching can be controlled using HTTP headers. Splitting js and css bundles into multple modules means updates don't require re-downloading and re-caching large files.

## PRPL pattern
 PRPL: acronymn to describe a pattern used to make web pages perfromant
- **P**ush (or preload) the most important resources
- **R**ender the initial route as soon as possible
- **P**re-cache remaining assets
- **L**azy load other routes and non-critical assets

## Performance budget

Performance is determined by page weight = how many total bytes of data need to travel over the internet to the browser. A performance budget may include limites on the total page weight, total image weight, no. of http requests, no. of external resources etc. Budget gives a metric to measure any new feature against + decisioning tool.

Useful tools: [Webpack performance options](https://webpack.js.org/configuration/performance/), [Lighthouse's LightWallet](https://web.dev/use-lighthouse-for-performance-budgets/) feature to test builds against perf budget.

Best practice metrics (based on a low-powered feature phone on 3G):
- Speed index under 3 seconds
- Time to Interactive (TTI) under 5 seconds
- Largest Contentful Paint (LCP) under 1 second
- Max Potential First Input Delay under 130ms
- Max Gzipped JS bundle under 170kb
- Total bundle size under 250kb

How to create realistic budget for own site? 
- Different budgets for different scenarios (e.g. mobile devices on slow network and desktop on fast networks)
- Do a performance audit first of own project and of competitors
- Set reasonable goals based on audit
- Test production build against perf budget (can test with different server and browsers configs). Check where bottlenecks are.

 Perf budgets are unique to each project and its requirements + can change over time
 
 ## Optimizing images
 
 Images have by far the largest impact on performance. 
 - **image quality**: The more detail the image has, the larger the data necessary to display it. Huge data savings & performance gains in reducing the quality of the images. [Squoosh](https://squoosh.app/): service to compress image files as much as possible. Shallow depth of field means better performance.
 - **image scaling**: it's possible to downscale an image by 87% to reduce complexity and then upscale the result by 115% to get an image of the same starting size with significant less complexity, without anyone noticing.
 - **image format**: image format or file type has direct impact on performance (JPG/JPEG, PNG, GIF, SVG, WebP). WebP: lossless and lossy image format with transparency created for the web, aims to replce JPG/JPEG - smaller image sizes than JPG/JPEG and PNG, however only supported by modern browsers (need to use JPG fallbacks for older browsers). Always use video instead of animated GIF.
 - **manual image optimization**: decide maximum visible size of the image - good rule is never go over 1920px width for full-bleed images. Scale images to maximum displayed size, so not to serve an image larger than displayed size. Experiment with compression in WebP and JPG, the lower the compression the better - good rule is around 75%. Run svgs through svgs compressors to ensure they are as simple as possible.
 - **automated image optimization**: dedicated tools for image optimization, [imagemin](https://www.npmjs.com/package/imagemin) and its plugins to compress different image formats, [Squoosh experimental CLI](https://www.npmjs.com/package/@squoosh/cli), [sharp](https://www.npmjs.com/package/sharp)(which provide whole suite of image optimization, transformation and modification). Advice: use sharp to generate different sized images if needed, then imagemin to minify the generated images.
 - **responsive images**: in order to deliver appropriately sized image every time use source sets (`srcset`) and `sizes`, so browser can pick correct image for each viewport. Sizes attribute defined the image size at any screen width using media queries. Always provide an image for the smallest possible screen (usually 320px) + make widest image 1920px + control displayed width for other images using sizes attribute & finding natural breakpoints. Try to limit yourself to 4/5 image sizes.
 - **lazy loading images**: native lazy loading supported by all modern browsers with the loading=lazy img attribute. For older browsers support: [lazysizes](https://github.com/aFarkas/lazysizes)

## Markup and content

- **Javascript**: minify to reduce size and uglify to improve code efficiency ([Uglify](https://www.npmjs.com/package/uglify-js), [Terser](https://www.npmjs.com/package/terser), [Webpack](https://webpack.js.org/guides/production/#minification)), code split and use ESM modules when possible. JS fuynctionality should be modularised as much as possible and split in different files: clear separation of concerns, can be loaded conditionally ( = perf benefits) and when one module updates not all JS need to be downloaded again. First load only critical Js, then necessary functionality, then lazy load the rest.
    - script tag is **render blocking**: `async` and `defer` keywords change this behaviour improving performance as browser parsing isn't paused while the script is being downloaded. **Async**: loading is asynchronous but execution is synchronous. **Defer**: loading is styll async (not render blocking) but execution is deferred until the HTML parsing is complete ( = same as placing `script` tag at the bottom of the `body`, except script is loaded async so better perf). Best practices: place `script` tags in `head`, use `async` as default, defer scripts that can be or that need fully built DOM.
    - Js modules can be lazy loaded using `import()`, so that they're loaded only when they're needed
- CSS: minify, post process, inline critical css, defer loading non-critical css. 
 
 

