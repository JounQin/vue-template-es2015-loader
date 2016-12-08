# vue-template-es2015-loader
compile vue template file specifically without `.vue` file but still using `vue-loader`

*This module is just a wrapper of `vue-loader/lib/template-compiler`.*

You need to add `vue-loader` into your (dev)dependencies, which is required to use inside.

With this package/loader, you could specific your template, script, style out of `.vue`, what means you can write your template in a `.pug` or other template engine.

Of course, you should handler the template loader by yourself.

## Example

If your are using `pug`, try to add a loader config as following:

``` js
{
  test: /\.pug$/,
  loader: `vue-template-es2015-loader!pug-html-loader?exports=false&pretty`,
  exclude: nodeModules
}
```

As you see, you should transform the `.pug` template by using `pug-html-loader` first, then `vue-template-es2015-loader` will handler the transformation from static html template to compiled render functions.
