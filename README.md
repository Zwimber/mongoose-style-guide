# Node.js Mongoose Style Guide

This is a guide for writing consistent and aesthetically pleasing mongoose models.
It is inspired by what is popular within the community, and flavored with some
personal opinions.

The style-guide style is largely based on [this style-guide](https://github.com/felixge/node-style-guide) and assumes knowledge of it.

This guide was created by [Woodland](https://woodl.nl/) and is
licensed under the [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)
license. You are encouraged to fork this repository and make adjustments
according to your preferences.

![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)

## Table of contents

### Object styling
* [Basics]('#basics')
* [CamelCase versus object]('#camelcase-versus-object')
* [Populateable guide]('#populate')

### Naming Conventions
* To be added

### Folder structuring
* [Suggestion for folder structure](#folder-structure)

## Object styling
### Basics

Basic structure of an exported schema.

```js
let Schema = require('mongoose').Schema
let SchemaObjectId 	= Schema.Types.ObjectId;

let SchemaMain = new Schema({
  ...schema content...
})

module.exports = SchemaMain;
```

Your schema's are summarised in another file. An suggestion for folder structure can be found later on. 

```js
let model = require('mongoose').model

let root = '<holds a reference to the root of project, alternatively describe path>'
let SchemaUser = require(root + '/models/user')

module.exports = {
	User     : model('user', SchemaUser),
}
```
User - capital since it is an class
user - lowecase, this will be the collection name in MongoDB, some versions will pluralize this


### CamelCase versus object

Content will be added.

### Populateable guide

Populate

*Right:*

```js
if (true) {
  console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
  console.log('losing');
}
```

