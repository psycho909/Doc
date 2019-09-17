```js
const babel = require('@babel/core');

const code = `[1, 2, 3, 4, [5, 6, [7, 8]]].flat(Infinity);`;
const ast = babel.transform(code, {
  presets: [
    [
      '@babel/preset-env',
      {
        useBuiltIns: 'usage',
        corejs: 3
      }
    ]
  ]
});
// 用core-js@3 来看看转码后的结果
console.log(ast.code);

// "use strict";

// require("core-js/modules/es.array.flat");

// require("core-js/modules/es.array.unscopables.flat");

// [1, 2, 3, 4, [5, 6, [7, 8]]].flat(Infinity);
```

