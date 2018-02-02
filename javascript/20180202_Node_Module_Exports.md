# Node modules: `module.exports` vs `exports`

## tl;dr
* `module.exports`: a variable gets returned from `require('module-name')`
* `module.exports` default value: `{}`, an empty object
* `exports`: just a reference to `module.exports`

## Caveat
* If you are not sure, always use `module.exports` over `exports`
* Do not override entire `exports`, instead add attributes:
```
// DON'T
exports = {
  log: function () {...}
}

// DO
exports.log = function () {...}
```
* **Why?** Since `exports` is just a reference to `module.exports`, replacing the entire `exports` wouldn't affect anything on `module.exports`, thus the module won't return expected `exports`.
