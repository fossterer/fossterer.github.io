# Web Development Series - Part 5 - Making sense of JavaScript ecosystem in 2026

* Web Applications are run in browsers. Every browser rendered HTML, evaliated JavaScript on their own terms until standard bodies like W3C, EcmaScript came up
* All along, web developers and companies built their own tools for their website/web application development workflows across various browsers
* *Babel*: Your JavaScript web application uses latest syntax standardized by EcmaScript Committee in 2010 but 60% (not all) of your users use IE11 released long ago. How can your code work? That's what Babel does. Utilizing CoreJS etc. let's this compiler put in polyfills so that when loaded in an old browser, an equivalent set of code replaces latest syntax you used during development
* *Webpack*: Weboack is a bundler. If your JavaScript code is split across multiple files and you want to import functions in one file in another, you used to use Webpack to manage the final output when this gets sent to a browser that has no idea what your "imports" mean. Later with EcmaScript modules, the imports became standardized but browsers before this didn't natively support it. Now you have *esbuild*
