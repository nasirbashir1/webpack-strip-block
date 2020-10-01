Webpack Strip Code
===================

Webpack loader to remove portion of code enclosed by the comment tags. It can be used to remove any code that you don't 
want in your production chunks / bundle. You can also pass mutliple markers in the loader based on some conditional logic.

### Example:

In your client js source files:

```javascript

var makeFoo(bar, baz) {
    // The code enclosed within the comment blocks would be removed
    /* REMOVE-INSTANCE-CHECKS-START */
    if (bar instanceof Bar !== true) {
        throw new Error('makeFoo: bar param is required and must be instance of Bar');
    }
    /* REMOVE-INSTANCE-CHECKS-END */

    // The code outside the comment blocks would remain as such
    return new Foo(bar, baz);
}

```

In your webpack config, specify the loader and custom comment tags to mark the start and end of the block to be removed from your code; you can add the options like this:

```javascript
module.exports = {
  rules: [
    {
      test: /\.js$/,
      enforce: 'pre',
      exclude: /(node_modules|bower_components|\.spec\.js)/,
      use: [
        {
          loader: 'webpack-strip-code',
          options: {
            choiceArray: [
              {
                start: 'REMOVE-INSTANCE-CHECKS-START',
                end: 'REMOVE-INSTANCE-CHECKS-START'
              },
              {
                start: 'ANOTHER-CUSTOM-COMMENT-START',
                end: 'ANOTHER-CUSTOM-COMMENT-END'
              }
            ]
          }
        }
      ]
    }
  ]
};
```
