## How to Add Script Tags in React

Using third party libraries is very common while developing apps for the web. The usual way is to install the NPM package of the library and import it for your use. 

But sometimes, the NPM package is unavailable or you have to include files directly from a CDN or external source. Adding `<script>` tags in the index.html file does not work every time and even if it does, it could cause issues as the website scales.


I faced a similar issue while adding Calendly import to my portfolio site and found an easy solution. But first, let's understand why exactly an error occurs when you add `<script>` tags in React components.

## Why it Throws an Error

React uses [React DOM](https://reactjs.org/docs/react-dom.html) to render JSX content on the web page. React DOM is a virtual DOM that resides on top of the original DOM. It only updates changed nodes from the DOM unlike the original DOM, which completely updates itself after every change.React DOM uses `createElement` to convert JSX into DOM elements.

The `createElement` function uses the `innerHTML` API to add changed nodes in the browser's original DOM. HTML5 specifications state that `<script>` tags are not executed if they are inserted with `innerHTML`. [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML#security_considerations) has explained the security reasons behind this.

As a result, The execution of the `<script>` tag throws an error in React.

## The Solution
The simplest solution is to add scripts directly into DOM using the `Document` interface provided by web APIs. We can use JavaScript's DOM manipulation methods to inject the `<script>` tag without React DOM interfering.

Here is what we have to do:

- At first, we get head and script tags from DOM.
- Then, we use the setAttribute method to add a new script.
- The modified script tag is appended to the head.

In React terms, the desired script has to be added to DOM when the component loads on the browser. React has a hook for such scenarios: `useEffect`. The whole process explained above can be wrapped inside the hook and triggered when the component renders for the first time or a new script is added.

In real world projects, we might want to add multiple scripts. Hence it's better to create a custom hook so we can call it multiple times with different source links.

Custom hooks are usually stored in a separate directory within the `/src` folder. Let's create a new file inside the `/src/hooks/` directory and name it `useExternalScripts.js`. Paste the following code in the file:

```jsx
import { useEffect } from 'react';

export default function useExternalScripts({ url }){
  useEffect(() => {
    const head = document.querySelector("head");
    const script = document.createElement("script");

    script.setAttribute("src", url);
    head.appendChild(script);

    return () => {
      head.removeChild(script);
    };
  }, [url]);
};
```

In a component where you want to add a new script, paste the following code:

```jsx
import useExternalScripts from "./hooks/useExternalScripts"

const Component = () => {
  useExternalScripts("https://www.scriptdomain.com/script")
  ...
}
```

A new script is appended to the head of the page whenever the component is mounted in the DOM. The script is removed when the component unmounts.

Don't use the `return` snippet if your script is used in multiple components throughout your app.The function returned by the hook is a cleanup function, which is executed when a component is unmounted. Hence we don't require it if we have to use the source at multiple places.

## Alternate Solution
Alternatively, you can use [react-helmet](https://www.npmjs.com/package/react-helmet) which manages changes within the `<head>` tag. The `<Helmet>` can take care of the script if it is placed inside of it.

```jsx
import { Helmet } from "react-helmet"

export default function Component() {
  return (
    <>
      <Helmet>
        <script
          src="https://www.myscripts.com/scripts"
          crossorigin="anonymous"
          async
        ></script>
      </Helmet>
      ...
    </>
  )
}
```

Don't forget to install react-helmet before you start your app!

## Wrapping Up
React uses `innerHTML` at the core to manipulate nodes on the browser's DOM. The `innerHTML` API doesn't support `<script>` tags for security reasons. Hence an error is thrown if you try to inject a `<script>` tag in a React component.

Adding a new script tag and directly appending it to the `<head>` element of the page is the easiest way to add `<script>` tags in the React app. react-helmet is a third party library that can be used to achieve the same thing by handling the `<head>` tag on every page.

I feel the custom hook version is better than using a third party library since we have full control over it. What do you think? Did you use any other method? Let me know down below!

If you found this blog helpful, consider sharing it on your social. You can read more blogs about web dev, open source and stuff I fix while developing apps on [my blog](https://clumsycoder.hashnode.dev/). Or if you want to say hi, I am most active on [Twitter](https://twitter.com/clumsy_coder).

Until then, happy debugging! ⛑