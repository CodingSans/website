---
title: accessor-pairs - Rules
layout: doc
edit_link: https://github.com/eslint/eslint/edit/master/docs/rules/accessor-pairs.md
rule_type: suggestion
---
<!-- Note: No pull requests accepted for this file. See README.md in the root directory for details. -->

# Enforces getter/setter pairs in objects (accessor-pairs)

It's a common mistake in JavaScript to create an object with just a setter for a property but never have a corresponding getter defined for it. Without a getter, you cannot read the property, so it ends up not being used.

Here are some examples:

```js
// Bad
var o = {
    set a(value) {
        this.val = value;
    }
};

// Good
var o = {
    set a(value) {
        this.val = value;
    },
    get a() {
        return this.val;
    }
};

```

This rule warns if setters are defined without getters. Using an option `getWithoutSet`, it will warn if you have a getter without a setter also.

## Rule Details

This rule enforces a style where it requires to have a getter for every property which has a setter defined.

By activating the option `getWithoutSet` it enforces the presence of a setter for every property which has a getter defined.

## Options

* `setWithoutGet` set to `true` will warn for setters without getters (Default `true`).
* `getWithoutSet` set to `true` will warn for getters without setters (Default `false`).

### setWithoutGet

Examples of **incorrect** code for the default `{ "setWithoutGet": true }` option:

```js
/*eslint accessor-pairs: "error"*/

var o = {
    set a(value) {
        this.val = value;
    }
};

var o = {d: 1};
Object.defineProperty(o, 'c', {
    set: function(value) {
        this.val = value;
    }
});
```

Examples of **correct** code for the default `{ "setWithoutGet": true }` option:

```js
/*eslint accessor-pairs: "error"*/

var o = {
    set a(value) {
        this.val = value;
    },
    get a() {
        return this.val;
    }
};

var o = {d: 1};
Object.defineProperty(o, 'c', {
    set: function(value) {
        this.val = value;
    },
    get: function() {
        return this.val;
    }
});

```

### getWithoutSet

Examples of **incorrect** code for the `{ "getWithoutSet": true }` option:

```js
/*eslint accessor-pairs: ["error", { "getWithoutSet": true }]*/

var o = {
    set a(value) {
        this.val = value;
    }
};

var o = {
    get a() {
        return this.val;
    }
};

var o = {d: 1};
Object.defineProperty(o, 'c', {
    set: function(value) {
        this.val = value;
    }
});

var o = {d: 1};
Object.defineProperty(o, 'c', {
    get: function() {
        return this.val;
    }
});
```

Examples of **correct** code for the `{ "getWithoutSet": true }` option:

```js
/*eslint accessor-pairs: ["error", { "getWithoutSet": true }]*/
var o = {
    set a(value) {
        this.val = value;
    },
    get a() {
        return this.val;
    }
};

var o = {d: 1};
Object.defineProperty(o, 'c', {
    set: function(value) {
        this.val = value;
    },
    get: function() {
        return this.val;
    }
});

```

## Known Limitations

Due to the limits of static analysis, this rule does not account for possible side effects and in certain cases
might not report a missing pair for a getter/setter that has a computed key, like in the following example:

```js
/*eslint accessor-pairs: "error"*/

var a = 1;

// no warnings
var o = {
    get [a++]() {
        return this.val;
    },
    set [a++](value) {
        this.val = value;
    }
};
```

Also, this rule does not disallow duplicate keys in object literals, and in certain cases with duplicate keys
might not report a missing pair for a getter/setter, like in the following example:

```js
/*eslint accessor-pairs: "error"*/

// no warnings
var o = {
    get a() {
        return this.val;
    },
    a: 1,
    set a(value) {
        this.val = value;
    }
};
```

The code above creates an object with just a setter for the property `"a"`.

See [no-dupe-keys](no-dupe-keys) if you also want to disallow duplicate keys in object literals.

## When Not To Use It

You can turn this rule off if you are not concerned with the simultaneous presence of setters and getters on objects.

## Further Reading

* [Object Setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set)
* [Object Getters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get)
* [Working with Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)

## Version

This rule was introduced in ESLint 0.22.0.

## Resources

* [Rule source](https://github.com/eslint/eslint/tree/master/lib/rules/accessor-pairs.js)
* [Documentation source](https://github.com/eslint/eslint/tree/master/docs/rules/accessor-pairs.md)
