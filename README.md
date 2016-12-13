# vue-template-es2015-loader
compile vue template file specifically without `.vue` file

*This module is inspired by [vue-template-loader](https://www.npmjs.com/package/vue-template-loader) and
[vue-loader](https://www.npmjs.com/package/vue-loader)*

You need to add `vue-template-compiler` into your (dev)dependencies, which is required to use inside. (Please make sure
it has the same version with your vue version)

With this package/loader, you could specific your template, script, style out of `.vue`, what means you can write your
template in a `.pug` or other template engine.

Of course, you should handler the template loader by yourself.

## Why not `.vue`

`.vue` is a great file type, and I love it, but not every IDE supports it very well. :(

So I make a decision to separate the three parts of `.vue` into normal `.html`, `.js`, `.css` (or other similar types).

And also there is a problem with `.vue`. [Parent component could not override style of children.](https://github.com/vuejs/vue-loader/issues/521)

## Why not `template` option

The `template` option requires Vue to compile it when app is running, what means you must use `vue/dist/vue.common.js`
instead of `vue.runtime.common.js`(`main` field in `package.json`). Then your bundle size will be a bit larger and your
app will be a bit slower than excluding the compiler and compiling it in advance.

That is what this module wants to do (same as the `vue-loader`).

## Configuration

If your are using `pug`, try to add a loader config as following:

``` js
{
  test: /\.pug$/,
  loader: `vue-template-es2015-loader!pug-html-loader?exports=false&pretty`,
  exclude: /node_modules/
}
```

As you see, you should transform the `.pug` template by using `pug-html-loader` first, then `vue-template-es2015-loader`
will handler the transformation from static html template to compiled render functions.

## Usage

The template file will be compiled to an function that can inject render function to a component options object which is
a valid vue component already:

``` js
{
  render: <Function>,
  staticRenderFns: <Array<Function>>
}
```

## Combine with styles and other options

``` js
// YourComponent.js

// You can also just import your style here when not using `css modules`
import classes from './index.styl'

export default require('./index.pug')({
  data() {
    return {
      // using css modules
      classes
    }
  }
  // `computed`, `methods` get here as normal
})
```

## Hot Reload Enabled!

Pass a component object to the compiled function enables hot-reload of template, because it combines template and
other vm options although it needs to be nested :(.

Ugly but useful :)

## What's the difference with `vue-template-loader`

Actually they are very similar, but `vue-template-es2015-loader` helps you to handle img source path automatically,
and transform the es2015 feature in template into es5 (Thanks to `vue-template-es2015-compiler` which uses `buble` to do
that).

This loader will respect the option of `vue-loader`.

---

Finally, we can use vue template without `.vue`! 

Please remember to handler the style files by yourself.
