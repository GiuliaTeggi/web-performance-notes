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
- **Browser tools**: Lighthouse (Chrome), network monitor and performance monitor in modern browsers
- **Hosted third-party tools**: PageSpeed Insights (Goodle), WebPage Test

Standard performance measurements: 

- **Time to first byte (TTFB)**: the time it takes for the browser to receive the first byte of the actual page it's looking for
- **First Paint**: 
- **Largest Contentful Paint**:
- **First Meaningful Paint**:
- **Time to Interactive**:

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
 
 

