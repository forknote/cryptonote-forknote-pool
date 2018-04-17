# BBS pool upgrade instruction for supporting Cryptonight v7

## Following changes are required, please read carefully
You can change the code manually by following the steps below if you are using non-bbscoin pool source <br>
Or cherry-pick this commit: <br>
https://github.com/bbscoin/bbscoin-pool/commit/3283344596c2231ebfe34708546f9deb487001c6


### Switch algorithm based on the block version
#### find following line in `lib/pool.js` 
```js
convertedBlob = cnUtil.convert_blob(shareBuffer);
```
Add following lines after it
```js
const majorVersion = convertedBlob[0];
hash = cryptoNight(convertedBlob, majorVersion >= 3 ? 1 : 0);
```

### Switch multi-hashing and cryptonote-util to bbscoin's own fork
#### replace following statements in `lib/pool.js` and `lib/utils.js`
```js
// Replace 
var multiHashing = require('multi-hashing');
// with 
var multiHashing = require('multi-hashing-bbscoin');

// Replace
var cnUtil = require('cryptonote-util');
// with 
var cnUtil = require('cryptonote-util-bbscoin');
```

### Update package.json
```js
// Replace the line with cryptonote-util with 
"cryptonote-util-bbscoin": "^1.0.3",

// Replace the line with multi-hashing with 
"multi-hashing-bbscoin": "^1.0.0",
```

### Run NPM install to install the new dependencies
`cryptonote-util-bbscoin` and `multi-hashing-bbscoin` only work with node v9.5.0+ <br>
Please update your node.js version before running npm install.
```
npm install
```

### Restart the node processes
