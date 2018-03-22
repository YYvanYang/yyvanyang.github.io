---
layout: post
title:  "ES2015 modules to CommonJS transform with Webstorm"
---

### Installation

```bash
npm install --save-dev babel-plugin-transform-es2015-modules-commonjs
```

### Usage

Via .babelrc (Recommended)
.babelrc

```js
// without options
{
  "plugins": ["transform-es2015-modules-commonjs"]
}

// with options
{
  "plugins": [
    ["transform-es2015-modules-commonjs", {
      "allowTopLevelThis": true
    }]
  ]
}
```

Via CLI

```bash
babel --plugins transform-es2015-modules-commonjs script.js
```

### Webstorm config with babel
在.babelrc中配置`transform-es2015-modules-commonjs`即可。


###### reference

[ES2015 modules to CommonJS transform](http://babeljs.io/docs/plugins/transform-es2015-modules-commonjs/)