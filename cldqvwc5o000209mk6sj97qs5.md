# Is Create React App Really Dead? What Are Other Options?

If you are active on tech Twitter, follow web-dev YouTubers, or read blogs, you know what I am talking about. For the past few months, there has been a lot of discussions about how `create-react-app`, the official tool to configure a react project, is out of date and needs to be replaced.

In today's blog, let us understand why everyone is against CRA, whether it is really that bad, and what other options we have on our plate.

This blog requires no prerequisites except some familiarity with the React.js ecosystem.

## How It Started

I am seeing similar content on my feed for the last few months. People started writing blogs and making videos saying CRA is dead and we should use Vite instead.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675535000965/bf87a322-6a85-4e67-a480-d76761b82048.png align="center")

Everything seemed fine until last week [Theo](https://twitter.com/t3dotgg) tweeted about filing a PR to remove CRA as a recommendation.

%[https://twitter.com/t3dotgg/status/1616933234478309378] 

And well, he did it! Soon after the tweet blew up and many developers agreed to him, he raised a PR.

%[https://github.com/reactjs/reactjs.org/pull/5487] 

This raised a question: Is CRA really as bad as developers are claiming? And if yes, what can we do about it?

## The Real Problems

`create-react-app` is the official tool for configuring React applications when you start a new project. It is maintained by the core React team responsible for the main React library.

The problem occurs when your app scales in size and has a lot of stuff to be taken care of. Not having a scalable structure makes your app slow and increases its rendering time.

### It is Unmaintained

The [create-react-app GitHub repository](https://github.com/facebook/create-react-app/) does not have active maintainers. Although few maintainers take care of minor updates and patches, nobody has been actively maintaining it since 2021.

%[https://github.com/facebook/create-react-app/discussions/11768] 

Hence around 1.5k issues are pending, and over 400 pulls are yet to be merged. This makes it hard to use it for larger projects that will scale enormously in the future.

### Overdependency on Webpack

CRA uses Webpack under the hood to set everything up. Webpack's main task is to pack everything into a single JavaScript bundle and ship it to the browser to execute.

However, using Webpack itself is a very tedious and frustrating task. We need to consider many decisions before configuring it. And thankfully, CRA did everything for us! It took care of all the Webpack configuration and removed all complexities, and gave us a ready-to-build react application in our hands!

Despite Webpack being useful, we can't ignore that it is tedious, slow, and old. While alternatives are using newer tools that are fast, minimal, and simple, CRA is still stuck on Webpack for providing backward compatibility. And CRA is still heavily dependent on Webpack.

### Lacks Many Features

CRA again falls back when supporting additional features out of the box. We don't use plain React anymore. We add different flavors to spice up things and make it more convenient, scalable, and robust. CRA does not support many such tools out of the box. We need to rely on third-party services or add it manually ourselves.

Developers today widely use TypeScript to typeface JavaScript applications. And as you might have guessed, CRA does not natively support TypeScript. You need to use a template while setting up the project.

Another library developers prefer Tailwind CSS. It is a utility-based CSS framework that provides a design system for websites. CRA requires CRACO, a wrapper around CRA, to install Tailwind CSS. Though it is simple to set up, there are many problems with it. Since CRA doesn't allow imports outside `/src`, loading `tailwind.config.js` to access the theme config with TypeScript becomes difficult.

### Better Alternatives

Using CRA is simple (and still is). This is best for beginners who might get overwhelmed by all the configuration and settings of starting a react project manually. You are only three commands away from having a React application ready.

But as React got popular, newer alternatives were introduced. These have new approaches to setting up React. They use more recent tools to do it and, overall, faster than CRA.

## What Else Can We Use?

There are three five main alternatives to vanilla React or CRA. You first need to decide the scope of your project and the utilities it needs and then choose a tool or framework.

If you want a CRA-like build tool that works exactly like CRA but resolves all of its problems, Vite is the way!

### Vite

[Vite](https://vitejs.dev/) is a recent build tool that is dominating the web-dev circle. It's on the third rank by usage and first by retention and interest in the state of JS 2022 survey.

Vite is *blazingly* fast. It uses SWC, which uses rust to build steps, so you get milliseconds response time while making changes to the app.

![The State of JS 2022 survey of build tools in which Vite usage has increased by 89%](https://cdn.hashnode.com/res/hashnode/image/upload/v1675533763763/398203e2-03b5-4520-9dd8-af2aebdae146.png align="center")

Vite supports a plethora of plugins to extend your build. It also supports TypeScript, PostCSS, and CSS modules out of the box, making it a production-ready build tool for React.

It also supports Svelte, Vue, Preact, Lit, and Vanilla JavaScript. In terms of react, it is an excellent replacement for CRA if you want a very similar experience to CRA. If your application doesn't require server side rendering and is a small or single page application, Vite is a must try!

### Server Side Generation

Server Side Generation, or SSG, is a website rendering approach that pre-renders the content and serves static HTML pages to the client.

The big players in SSG are [Gatsby](https://www.gatsbyjs.com/) and [Astro](https://astro.build/). They both are best for scenarios where you need to create a static side that doesn't rely on dynamic content. So if you are building a blog, landing pages, or portfolio marketing, SSG is the way to go.

Unlike Vite, Astro and Gatsby add their flavor on top of React, so the code you write slightly changes compared to CRA/Plain React.

### Server Side Rendering

Server Side Rendering, or SSR, generates static content on the server and sends it to the client for every request. It drastically improves the overall loading time, which helps increase Core Web Vitals scores and get better website SEO results.

Again we have two frameworks to talk about**—** [Next.js](https://nextjs.org/) and [Remix](https://remix.run/). Both have a similar approach to how a project is set up, but there are obviously a few differences. Starting with similarities, both frameworks support file-based routing and are used for dynamic content generation. Talking about differences, Next uses React server components while Remix does not. Nested routes work in beta for Next.js, whereas Remix has a stable release. Remix uses ESBuild as a build tool, while NExt uses rust-based SWC.

Suppose your requirements include a multi-page application heavily dependent on dynamic data and complex routes. In that case, SSR is the way to go. Like SSG frameworks, both Next and Remix are built on top of React, so you need to spend some extra time learning them.

## My Views

We should not force anyone to use a particular tool or framework. React by itself is still a great choice for a framework, and it is changing with changing needs over time as well.

If you are a beginner and creating simple projects to validate your learnings, there is no harm in using CRA. Vite is useful for a CRA-like experience with better support and speed. Use it when you are creating small or medium scaled single page applications.

Astro and Gatsby should be considered when your content won't change often. This makes it useful in use cases like blogs, portfolios, landing pages, etc.

Moving towards Next.js or Remix would be best once you become comfortable with React (with or without CRA). These frameworks give much flexibility and scalability to create robust front-end applications.

## Wrapping Up!

It feels good to have multiple options available to us. We can use the best tool, library, or framework that satisfies our project's scope. Also, having healthy competition is always good for the market.

These were my views, and yours might vary; that's absolutely fine! I would love to hear yours. I am most active on [Twitter](https://twitter.com/clumsy_coder) and [Showwcase](https://clumsycoder.showwcase.com/) if you want to say hi!

I write about web, open-source, technical writing, and my general experiences with development. I also write about Web3 at [The Dapp List](https://thedapplist.com/). You can follow me on [Twitter](https://twitter.com/clumsy_coder) or subscribe to my newsletter below to have the latest update on everything I am up to.

Until then, Happy tooling!

## References

* [Don't Use Create React App in 2023](https://youtu.be/o9TJWEPc0Lk)
    
* [Stop Using Create React App](https://youtu.be/kvkAoCbTM3Q)
    
* I was almost finished with the blog when Fireship posted a video. [7 better ways to create a React app](https://youtu.be/2OTq15A5s0Y)