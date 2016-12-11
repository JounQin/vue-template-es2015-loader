# vue-template-es2015-loader
compile vue template file specifically without `.vue` file but still using `vue-loader`

*This module is just a wrapper of `vue-loader/lib/template-compiler`.*

You need to add `vue-loader` into your (dev)dependencies, which is required to use inside.

With this package/loader, you could specific your template, script, style out of `.vue`, what means you can write your
template in a `.pug` or other template engine.

Of course, you should handler the template loader by yourself.

## Why not just use `template` option

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

The template file will be compiled to an object like following which is a valid vue component already:

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

export default {
  ...require('./index.pug'),
  data() {
    return {
      // using css modules
      classes
    }
  }
  // `computed`, `methods` get here as normal
}
```

---

Finally, we can use vue template without `.vue`! 

Please remember to handler the style files by yourself.
