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

Standard performance measurements: First Paint, Largest Contentful Paint, First Meaningful Paint, Time to Interactive.

Time to first byt (TTFB): the time it takes for the browser to receive the first byte of the actual page it's looking for

## http2 

Files and data sent between browser and server via http ( Hypertext Transfer Protocol). Two versions of http: 
- Old http/1.1 : all files requested by browser are downloaded synchronously, one at a time. To get around this browsers usually open 6 parallel connections, but, in addition to time + resources needed to set and manage multiple connections, first file still holds back the rest of the files from downloading ("head of line" blocking).
- http2: multiplexing, that is browser can download files at the same time over one connection and each file is independent of the others.



