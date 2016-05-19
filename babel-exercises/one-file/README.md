# Transpile, concatenate and watch

By the end of this lesson you should be able to setup an npm script that:

- transpiles all files in the `js` directory using es2015
- concatenates them all to a file named `dist/main.js`
- watches for changes and continually updates when you save a file

## Why is this so cool?

The #1 reason to use babel is to ensure that your es2015 (and beyond) code works in all modern browsers, not just the latest evergreen browsers.  Another benefit is that the babel toolchain can also make it a bit nicer to develop by concatenating all of your files into one after it transpiles them.

Of course there are other tools that can concatenate JS files, such as gulp/gulp-concat, but if the only thing you need to do is transpile and concatenate, babel-cli makes it very straightforward.

## Activity

Go to the [Babel CLI Manual](https://github.com/thejameskyle/babel-handbook/blob/master/translations/en/user-handbook.md) and also our [Babel Intro](../01_ECMAScript.md) and figure out how to:

- install `babel`, `babel-cli` and `babel-preset-es2015` _locally_ to your project (not globally to your computer)
- write a command line statement that transpiles everything from `js` and outputs it to `dist/main.js`
  - NOTE: they don't have an exact example of this, but you can figure it out :)
- add and configure the `.babelrc` file to add `babel-preset-es2015`
- add a flag to watch for changes to files

Then update index.html to just refer to that one file.
