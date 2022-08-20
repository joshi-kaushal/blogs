## Three Ways to Remove Console Statements From Production Build

Whether you are a beginner JavaScript Developer or an experienced developer working in the biggest organization in the world, you can't deny using console statements to debug your code. Even though nothing is wrong with doing it, you might expose some sensitive data to end users if you forget to remove those before committing the code.

In this quick article, let us discuss different ways by which you can prevent log statements from going into the production code.

## Overriding `console` Methods
This simplest practice removes console statements from the production build, leaving you with a great debugging tool during development.

In this method, we check the node environment of the app and override console functions with empty functions if the environment is not `development`. By doing this, you can still use console statements if the app is in development mode.

Paste the following code into the entry file of your application. In most cases, it would be named `index.js` or `app.js`.

```js
if (process.env.NODE_ENV !== "development") {
   console.log = () => {};
   console.debug = () => {};
   console.info = () => {};
   console.warn= () => {};
 }
```

> If you are using React or Next, you can paste this in `src/index.js` or `pages/_app.js`, respectively.


## ES-Lint 
If you are using ESLint configuration, you can add `no-console` rule to ensure that console statements are not committed to the version control. You can even exclude some statements (like `.warn()` or `.error()`) if you wish. 

However, you can follow this method only if you have integrated ESLint with your app. If you are using React or Next, follow [this article](https://clumsycoder.hashnode.dev/automate-eslint-and-prettier-for-react) to integrate ESLint, Prettier and Husky in your app.

If you have ESLint, add the following rule in the `.eslintrc.json` file.
```json
{
    "rules": {
        "no-console": "error",
    }
}
```
This rule will throw an error if a console statement is encountered. You can change the behavior by replacing `error` with `warning`. Similarly, you can exclude specific methods by adding an `allow` option, like this:
```json
{
    "rules": {
        "no-console": ["error", { allow: ["warn", "error"] }],
    }
}
```

This will allow `console.warn()` and `console.error()` but throw an error in other cases. You can read more about this rule on their [docs](https://eslint.org/docs/latest/rules/no-console).

## Using a Babel Plugin
A plugin called `babel-plugin-transform-remove-console` removes all console statements from your application. All you need to do is install it and add it to the `.babelrc` file.

Install this plugin by NPM or Yarn like this:
```bash
# NPM
npm i babel-plugin-transform-remove-console

# Yarn
yarn add babel-plugin-transform-remove-console
```

Then open `.babelrc` and paste the following. If the file doesn't exist, create a new file with the same name in the root directory.

```json
{
  "env": {
    "production": {
      "plugins": ["transform-remove-console"]
    }
  }
}
```
You can find more information about this library at the [NPM repository](https://www.npmjs.com/package/babel-plugin-transform-remove-console#via-babelrc-recommended).

## Wrapping Up!
The console statements are often not taken seriously. Removing them from production is always a best practice while building apps. But it could cause performance or security issues if you mishandle them. 

If you found this article helpful, share this within your socials so everyone can follow best practices.

I write blogs about web development, open source, and personal experiences. Make sure to follow me on [Hashnode](https://hashnode.com/@clumsycoder) for the latest article on your feed. If you want to say hi or need help with frontend or technical writing, catch me on [Twitter](https://twitter.com/clumsy_coder), [Showwcase](https://www.showwcase.com/clumsycoder), and [Peerlist](https://peerlist.io/clumsycoder). 

Until then, happy logging! 👨‍💻