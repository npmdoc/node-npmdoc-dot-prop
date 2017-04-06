# api documentation for  [dot-prop (v4.1.1)](https://github.com/sindresorhus/dot-prop#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-dot-prop.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-dot-prop) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-dot-prop.svg)](https://travis-ci.org/npmdoc/node-npmdoc-dot-prop)
#### Get, set, or delete a property from a nested object using a dot path

[![NPM](https://nodei.co/npm/dot-prop.png?downloads=true)](https://www.npmjs.com/package/dot-prop)

[![apidoc](https://npmdoc.github.io/node-npmdoc-dot-prop/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-dot-prop_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-dot-prop/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-dot-prop/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-dot-prop/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Sindre Sorhus",
        "email": "sindresorhus@gmail.com",
        "url": "sindresorhus.com"
    },
    "bugs": {
        "url": "https://github.com/sindresorhus/dot-prop/issues"
    },
    "dependencies": {
        "is-obj": "^1.0.0"
    },
    "description": "Get, set, or delete a property from a nested object using a dot path",
    "devDependencies": {
        "ava": "*",
        "matcha": "^0.7.0",
        "xo": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "a8493f0b7b5eeec82525b5c7587fa7de7ca859c1",
        "tarball": "https://registry.npmjs.org/dot-prop/-/dot-prop-4.1.1.tgz"
    },
    "engines": {
        "node": ">=4"
    },
    "files": [
        "index.js"
    ],
    "gitHead": "49f0809db1201f2cf13735de4f3631191a692658",
    "homepage": "https://github.com/sindresorhus/dot-prop#readme",
    "keywords": [
        "obj",
        "object",
        "prop",
        "property",
        "dot",
        "path",
        "get",
        "set",
        "delete",
        "del",
        "access",
        "notation",
        "dotty"
    ],
    "license": "MIT",
    "maintainers": [
        {
            "name": "sindresorhus",
            "email": "sindresorhus@gmail.com"
        }
    ],
    "name": "dot-prop",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/sindresorhus/dot-prop.git"
    },
    "scripts": {
        "bench": "matcha bench.js",
        "test": "xo && ava"
    },
    "version": "4.1.1",
    "xo": {
        "esnext": true
    }
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module dot-prop](#apidoc.module.dot-prop)
1.  [function <span class="apidocSignatureSpan">dot-prop.</span>delete (obj, path)](#apidoc.element.dot-prop.delete)
1.  [function <span class="apidocSignatureSpan">dot-prop.</span>get (obj, path, value)](#apidoc.element.dot-prop.get)
1.  [function <span class="apidocSignatureSpan">dot-prop.</span>has (obj, path)](#apidoc.element.dot-prop.has)
1.  [function <span class="apidocSignatureSpan">dot-prop.</span>set (obj, path, value)](#apidoc.element.dot-prop.set)



# <a name="apidoc.module.dot-prop"></a>[module dot-prop](#apidoc.module.dot-prop)

#### <a name="apidoc.element.dot-prop.delete"></a>[function <span class="apidocSignatureSpan">dot-prop.</span>delete (obj, path)](#apidoc.element.dot-prop.delete)
- description and source-code
```javascript
delete(obj, path) {
		if (!isObj(obj) || typeof path !== 'string') {
			return;
		}

		const pathArr = getPathSegments(path);

		for (let i = 0; i < pathArr.length; i++) {
			const p = pathArr[i];

			if (i === pathArr.length - 1) {
				delete obj[p];
				return;
			}

			obj = obj[p];

			if (!isObj(obj)) {
				return;
			}
		}
	}
```
- example usage
```shell
...

// has
dotProp.has({foo: {bar: 'unicorn'}}, 'foo.bar');
//=> true

// deleter
const obj = {foo: {bar: 'a'}};
dotProp.delete(obj, 'foo.bar');
console.log(obj);
//=> {foo: {}}

obj.foo.bar = {x: 'y', y: 'x'};
dotProp.delete(obj, 'foo.bar.x');
console.log(obj);
//=> {foo: {bar: {y: 'x'}}}
...
```

#### <a name="apidoc.element.dot-prop.get"></a>[function <span class="apidocSignatureSpan">dot-prop.</span>get (obj, path, value)](#apidoc.element.dot-prop.get)
- description and source-code
```javascript
get(obj, path, value) {
		if (!isObj(obj) || typeof path !== 'string') {
			return value === undefined ? obj : value;
		}

		const pathArr = getPathSegments(path);

		for (let i = 0; i < pathArr.length; i++) {
			if (!Object.prototype.propertyIsEnumerable.call(obj, pathArr[i])) {
				return value;
			}

			obj = obj[pathArr[i]];

			if (obj === undefined || obj === null) {
				// 'obj' is either 'undefined' or 'null' so we want to stop the loop, and
				// if this is not the last bit of the path, and
				// if it did't return 'undefined'
				// it would return 'null' if 'obj' is 'null'
				// but we want 'get({foo: null}, 'foo.bar')' to equal 'undefined', or the supplied value, not 'null'
				if (i !== pathArr.length - 1) {
					return value;
				}

				break;
			}
		}

		return obj;
	}
```
- example usage
```shell
...

## Usage

'''js
const dotProp = require('dot-prop');

// getter
dotProp.get({foo: {bar: 'unicorn'}}, 'foo.bar');
//=> 'unicorn'

dotProp.get({foo: {bar: 'a'}}, 'foo.notDefined.deep');
//=> undefined

dotProp.get({foo: {bar: 'a'}}, 'foo.notDefined.deep', 'default value');
//=> 'default value'
...
```

#### <a name="apidoc.element.dot-prop.has"></a>[function <span class="apidocSignatureSpan">dot-prop.</span>has (obj, path)](#apidoc.element.dot-prop.has)
- description and source-code
```javascript
has(obj, path) {
		if (!isObj(obj) || typeof path !== 'string') {
			return false;
		}

		const pathArr = getPathSegments(path);

		for (let i = 0; i < pathArr.length; i++) {
			if (isObj(obj)) {
				if (!(pathArr[i] in obj)) {
					return false;
				}

				obj = obj[pathArr[i]];
			} else {
				return false;
			}
		}

		return true;
	}
```
- example usage
```shell
...
//=> {foo: {bar: 'b'}}

dotProp.set(obj, 'foo.baz', 'x');
console.log(obj);
//=> {foo: {bar: 'b', baz: 'x'}}

// has
dotProp.has({foo: {bar: 'unicorn'}}, 'foo.bar');
//=> true

// deleter
const obj = {foo: {bar: 'a'}};
dotProp.delete(obj, 'foo.bar');
console.log(obj);
//=> {foo: {}}
...
```

#### <a name="apidoc.element.dot-prop.set"></a>[function <span class="apidocSignatureSpan">dot-prop.</span>set (obj, path, value)](#apidoc.element.dot-prop.set)
- description and source-code
```javascript
set(obj, path, value) {
		if (!isObj(obj) || typeof path !== 'string') {
			return;
		}

		const pathArr = getPathSegments(path);

		for (let i = 0; i < pathArr.length; i++) {
			const p = pathArr[i];

			if (!isObj(obj[p])) {
				obj[p] = {};
			}

			if (i === pathArr.length - 1) {
				obj[p] = value;
			}

			obj = obj[p];
		}
	}
```
- example usage
```shell
...
//=> 'default value'

dotProp.get({foo: {'dot.dot': 'unicorn'}}, 'foo.dot\\.dot');
//=> 'unicorn'

// setter
const obj = {foo: {bar: 'a'}};
dotProp.set(obj, 'foo.bar', 'b');
console.log(obj);
//=> {foo: {bar: 'b'}}

dotProp.set(obj, 'foo.baz', 'x');
console.log(obj);
//=> {foo: {bar: 'b', baz: 'x'}}
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
