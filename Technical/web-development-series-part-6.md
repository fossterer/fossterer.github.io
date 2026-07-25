# Web Development Series - Part 6 - Why is there so much tooling in the JavaScript space?

* For a Java/Python/any backend application, the target environment is a controlled single JRE/Python Runtime/something similar
* For JavaScript application, it's your user's browser! So which version of Chrome/Firefox/Edge/Internet Explore/Mobile browser/what else should your code work on? Also, you don't have control on what settings the user made to their browser. Do you?
* That's for runtime. Now to the compile-time and development time. All your users' browsers care about is JavaScript version they support. But you want to write Typescript or latest JS only, once. So the end compiler/transpiler wouldn't do type enforcement, validation etc. like `javac` etc. This *linting* is treated as its own step separate from *compilation* unlike in Java etc.
* Hence the plethora of tools. See [this thread](https://news.ycombinator.com/item?id=28862454)
* Continuing from the previous day's [post](web-development-series-part-5.md), see [this thread](https://news.ycombinator.com/item?id=28861433) on why you need both 'esbuild' and 'typescript' packages in your toolchain despite one appearing to be already doing the work of the other
* Now coming to the **Vite**land, *oxlint* and *oxfmt* are the optional equivalents to *eslint* and *prettier* which need to be installed on their own
* Every tool we named here and in the previous day's [post](web-development-series-part-5.md) can be configured through their own configuration files. That's why we see - tsconfig.json, .eslintrc, .prettierrc and so on all splattered in the root directory
* While many/all of these tools are optional, the one top-level tool where you trigger them from, along with managing dependencies is, in 2026, NPM. This is configured as *package.json* at the root directory. Treat this as equivalent to *pom.xml* in the Java land
* In *package.json*, you mention dependencies as well as *scripts* which are shortcuts to invoke all our optional tools
* Alternatives such as PNPM exist