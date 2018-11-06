# parse-git-config [![NPM version](https://img.shields.io/npm/v/parse-git-config.svg?style=flat)](https://www.npmjs.com/package/parse-git-config) [![NPM monthly downloads](https://img.shields.io/npm/dm/parse-git-config.svg?style=flat)](https://npmjs.org/package/parse-git-config) [![NPM total downloads](https://img.shields.io/npm/dt/parse-git-config.svg?style=flat)](https://npmjs.org/package/parse-git-config) [![Linux Build Status](https://img.shields.io/travis/jonschlinkert/parse-git-config.svg?style=flat&label=Travis)](https://travis-ci.org/jonschlinkert/parse-git-config)

**Forked from [jonschlinkert/parse-git-config](https://github.com/jonschlinkert/parse-git-config), But the origin using `util.promisify` which requires Node.js 8, so this one is compatible with node >= 6**

See [the PR](https://github.com/jonschlinkert/parse-git-config/pull/9)

> Parse `.git/config` into a JavaScript object. sync or async.

Please consider following this project's author, [Jon Schlinkert](https://github.com/jonschlinkert), and consider starring the project to show your :heart: and support.

## Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm install --save parse-git-config
```

## Usage

```js
const parse = require('parse-git-config')

// sync
const config = parse.sync()

// or async
parse(function(err, config) {
  // do stuff with err/config
})
```

**Custom path and/or cwd**

```js
parse.sync({ cwd: 'foo', path: '.git/config' })

// async
parse({ cwd: 'foo', path: '.git/config' }, function(err, config) {
  // do stuff
})
```

**Example result**

Config object will be something like:

```js
{ core:
   { repositoryformatversion: '0',
     filemode: true,
     bare: false,
     logallrefupdates: true,
     ignorecase: true,
     precomposeunicode: true },
  'remote "origin"':
   { url: 'https://github.com/jonschlinkert/parse-git-config.git',
     fetch: '+refs/heads/*:refs/remotes/origin/*' },
  'branch "master"': { remote: 'origin', merge: 'refs/heads/master', ... } }
```

## API

### [parse](index.js#L35)

Asynchronously parse a `.git/config` file. If only the callback is passed, the `.git/config` file relative to `process.cwd()` is used.

**Params**

- `options` **{Object|String|Function}**: Options with `cwd` or `path`, the cwd to use, or the callback function.
- `callback` **{Function}**: callback function if the first argument is options or cwd.
- `returns` **{Object}**

**Example**

```js
parse(function(err, config) {
  if (err) throw err
  // do stuff with config
})
```

### [.sync](index.js#L64)

Parse the given

**Params**

- `options` **{Object|String}**: Options with `cwd` or `path`, or the cwd to use.
- `returns` **{Object}**

**Example**

```js
parse
  .promise({ path: '/path/to/.gitconfig' })
  .then(config => console.log(config))
```

### [.sync](index.js#L98)

Synchronously parse a `.git/config` file. If no arguments are passed, the `.git/config` file relative to `process.cwd()` is used.

**Params**

- `options` **{Object|String}**: Options with `cwd` or `path`, or the cwd to use.
- `returns` **{Object}**

**Example**

```js
const config = parse.sync()
```

### [.expandKeys](index.js#L149)

Returns an object with only the properties that had ini-style keys converted to objects.

**Params**

- `config` **{Object}**: The parsed git config object.
- `returns` **{Object}**

**Example**

```js
const config = parse.sync({ path: '/path/to/.gitconfig' })
const obj = parse.expandKeys(config)
```

### .keys examples

Converts ini-style keys into objects:

**Example 1**

```js
const parse = require('parse-git-config')
const config = {
  'foo "bar"': { doStuff: true },
  'foo "baz"': { doStuff: true }
}

console.log(parse.keys(config))
```

Results in:

```js
{
  foo: {
    bar: { doStuff: true },
    baz: { doStuff: true }
  }
}
```

**Example 2**

```js
const parse = require('parse-git-config')
const config = {
  'remote "origin"': {
    url: 'https://github.com/jonschlinkert/normalize-pkg.git',
    fetch: '+refs/heads/*:refs/remotes/origin/*'
  },
  'branch "master"': {
    remote: 'origin',
    merge: 'refs/heads/master'
  },
  'branch "dev"': {
    remote: 'origin',
    merge: 'refs/heads/dev',
    rebase: true
  }
}

console.log(parse.keys(config))
```

Results in:

```js
{
  remote: {
    origin: {
      url: 'https://github.com/jonschlinkert/normalize-pkg.git',
      fetch: '+refs/heads/*:refs/remotes/origin/*'
    }
  },
  branch: {
    master: {
      remote: 'origin',
      merge: 'refs/heads/master'
    },
    dev: {
      remote: 'origin',
      merge: 'refs/heads/dev',
      rebase: true
    }
  }
}
```

## About

<details>
<summary><strong>Contributing</strong></summary>

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](../../issues/new).

</details>

<details>
<summary><strong>Running Tests</strong></summary>

Running and reviewing unit tests is a great way to get familiarized with a library and its API. You can install dependencies and run tests with the following command:

```sh
$ npm install && npm test
```

</details>

<details>
<summary><strong>Building docs</strong></summary>

_(This project's readme.md is generated by [verb](https://github.com/verbose/verb-generate-readme), please don't edit the readme directly. Any changes to the readme must be made in the [.verb.md](.verb.md) readme template.)_

To generate the readme, run the following command:

```sh
$ npm install -g verbose/verb#dev verb-generate-readme && verb
```

</details>

### Related projects

You might also be interested in these projects:

- [git-user-name](https://www.npmjs.com/package/git-user-name): Get a user's name from git config at the project or global scope, depending on… [more](https://github.com/jonschlinkert/git-user-name) | [homepage](https://github.com/jonschlinkert/git-user-name "Get a user's name from git config at the project or global scope, depending on what git uses in the current context.")
- [git-username](https://www.npmjs.com/package/git-username): Get the username from a git remote origin URL. | [homepage](https://github.com/jonschlinkert/git-username 'Get the username from a git remote origin URL.')
- [parse-author](https://www.npmjs.com/package/parse-author): Parse an author, contributor, maintainer or other 'person' string into an object with name, email… [more](https://github.com/jonschlinkert/parse-author) | [homepage](https://github.com/jonschlinkert/parse-author "Parse an author, contributor, maintainer or other 'person' string into an object with name, email and url properties following npm conventions.")
- [parse-authors](https://www.npmjs.com/package/parse-authors): Parse a string into an array of objects with `name`, `email` and `url` properties following… [more](https://github.com/jonschlinkert/parse-authors) | [homepage](https://github.com/jonschlinkert/parse-authors 'Parse a string into an array of objects with `name`, `email` and `url` properties following npm conventions. Useful for the `authors` property in package.json or for parsing an AUTHORS file into an array of authors objects.')
- [parse-github-url](https://www.npmjs.com/package/parse-github-url): Parse a github URL into an object. | [homepage](https://github.com/jonschlinkert/parse-github-url 'Parse a github URL into an object.')
- [parse-gitignore](https://www.npmjs.com/package/parse-gitignore): Parse a gitignore file into an array of patterns. Comments and empty lines are stripped. | [homepage](https://github.com/jonschlinkert/parse-gitignore 'Parse a gitignore file into an array of patterns. Comments and empty lines are stripped.')

### Contributors

| **Commits** | **Contributor**                                   |
| ----------- | ------------------------------------------------- |
| 51          | [jonschlinkert](https://github.com/jonschlinkert) |
| 1           | [sam3d](https://github.com/sam3d)                 |
| 1           | [js-n](https://github.com/js-n)                   |

### Author

**Jon Schlinkert**

- [linkedin/in/jonschlinkert](https://linkedin.com/in/jonschlinkert)
- [github/jonschlinkert](https://github.com/jonschlinkert)
- [twitter/jonschlinkert](https://twitter.com/jonschlinkert)

### License

Copyright © 2018, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT License](LICENSE).

---

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.6.0, on February 14, 2018._
