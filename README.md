
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
* [Basics](#basics)
* [CamelCase versus object](#camelcase-versus-object)
* [Populateable guide](#populate)
* [Storing history](#history)

### Naming Conventions
* To be added

### Folder structuring
* [Suggestion for folder structure](#folder-structure)

## Object styling
### Basics

Basic structure of an exported schema. Avoid specifying more than one schema per file.

```js
// (0) Require
let Schema = require('mongoose').Schema
let SchemaObjectId 	= Schema.Types.ObjectId;

// (1) Define object
let SchemaMain = new Schema({
	// Schema
})

// (2) Pre/post hooks
SchemaMain.pre('save', function(next) {
	next()
})

// (3) Methods
SchemaMain.methods.logThis = function() {
	console.log('This is a reference to the instance', this)
}

// (4) Statics
SchemaMain.statics.logModel = function() {
	console.log('This is a reference to the model', this)
}

// (5) Export
module.exports = SchemaMain;
```

Your schema's are summarised in another file. An suggestion for folder structure can be found later on. 

```js 
let model = require('mongoose').model
let SchemaUser = require(root + '/path/to/models/user')

module.exports = {
	User: model('user', SchemaUser),
}
```

**User** is a class so it is capitalised
**user** is a collection name in MongoDB so it is lowercase (some mongoose versions will pluralize this)

### Property naming

For property names always use camelCase. Try to order parts of the word from more to lesser important. If we want to have a property that stores data of an profile picture we suggest naming that property: "pictureProfile". 

This might seem counterintuitive but this standardised way of property naming has several advantages:

 1. Easy refactoring to new namespace
 2. Readable
 3. Consistent, which makes it easy to guess variable names
 
TLDR; tips when naming properties:

1. Use camelCase
2. Order camelCase parts from most to least important 
 (*do:* nameFirst, *don't*: firstName)
3. Don't restate the current model name
4. Be descriptive even though MongoDB favours short property names
5. Watch for reserved words

*Note: After this example we suggest an alternative way for storing username*
```js
// Good boy example
let user = {
	nameFirst: 'Tim',
	nameMiddle: null,
	nameLast: 'L',
	email: 'user@example.com',
	emailSettings: {},
	
	pictureProfile: '/url/to/profile.jpg',
	pictureBanner: '/url/to/banner.jpg',
	active: true,
}

// Bad boy example
let user = {
	name_first: 'Tim',					// (1) Not using camelCase
	middleName: null,					// (2) Wrong order of elements 
	userNameLast: 'L',					// (3) Restated name of model 
	e: 'user@example.com',				// (4) Not descriptive
	options: {},						// (5) Using reserved property names
	
	picture_profile: '/url/to/profile.jpg',	// (1) Not using camelCase
	banner_picture: '/url/to/profile.jpg',	// (2) Wrong order of elements
	userActive: true, 						// (3) Restated name of model
}
```
**Exception 0 - When properties need numbers**
Sometimes 
it is however [allowed](https://www.w3resource.com/slides/json-style-guide.php) to use numbers as a key when defining a map.
```js
// True camelCase example
let image = {
	icon32 : SchemaImage,
	icon64 : SchemaImage,
}

// Intermediate option
let image = {
	icon: {
		'32': SchemaImage,
		'64': SchemaImage,
	}
}

// Allowed for readability
let image = {
	icon_32: SchemaImage,
	icon_64: SchemaImage,
}

// Renaming, but you'll lose information and will run out of names quick 
// [xs, sm, md, lg, xl]
let image = {
	icon_xs: SchemaImage,
	icon_xl: SchemaImage,
}
```

### CamelCase versus object structuring
Often you are confronted with a tradeof between *flat* and *structured* JSON. Consider the following two representations:

```js
// Flat JSON
let user = {
    nameFirst: '',
    nameMiddle: '',
    nameLast: '',
}
```

```js
// Structured JSON (stringified 43 characters
let user = {
    name: {
        first: '', 
        middle: '',
        last: '',
    }
}
```
Flat structure seems more concise (please note that for a computer it is more lengthy!) and is usually advised: 
Structured JSON seems more verbose however it gives us several advantages:

 - Clear grouping of properties
	 - Extra advantage: shorter select objects
	 - Extra advantage: keep properties together (MongoDB does not preserve key order)
 - Easy to export and reuse 


We therefore suggest to **avoid all camelCase for these kinds of situations** where there is a clear parent-child relation. 

**Exception 0 - Intermediate properties are not used, and never will be used**
In rare cases where you want to be very descriptive and are not interested in using the intermediate fields using camelCase can be useful.

```js
// Before 
let user = {
	ageVerificationPictureUploadCompleted: true 
}

// After de-camelCase-ization
let user = {
	age: {
		verification: {
			picture: {
				upload: {
					completed: true
				}
			}
		}
	}
}
```
*Note: in this example it is very unlikely you would not want to use any of the intermediate properties (e.g. we might a place to store the picture at age.verification.picture.value)*

**Exception 1 - Intermediate property makes no sense**
In cases where you want to be very descriptive and are not interested in using the intermediate fields using camelCase can be useful.

```js
// Before 
let user = {
	roomLivingTelevisionCount: 1 
}

// After de-camelCase-ization
let user = {
	room: {
		living: {
			television: {
				count: 1
			}
		}
	}
}
```

**Exception 1 - Whenever there is great advantage in using the root property**
 --> sorting of object keys is not always preserved
 
 --> history
 
 --> linking both ways

var mainSchema 		= new Schema({

	total: { 
		type: Number, 
		default: 0,
	},
	
	livingRoom: {
		total:    				{ type: Number, default: 0, set: set },

		twoPersonSofa: 			{ type: Number, default: 0, set: set },
		threePersonSofa: 		{ type: Number, default: 0, set: set },
		televisionLivingRoom: 	{ type: Number, default: 0, set: set },
		smallFurniture: 		{ type: Number, default: 0, set: set },
		sideTable: 				{ type: Number, default: 0, set: set },
		secretary: 				{ type: Number, default: 0, set: set },
		plants: 


**Exception 2 - Reserved names**

Often we like to store when, and by whom an property is edited:
```js
// Before 
let user = {
	pincode: {
		value: '1234',
		setOn: '<Date>' 
		setBy: '<UserReference>'
	}
}

// After de-camelCase-ization
let user = {
	pincode: {
		value: '1234',
		set: {
			on: '<Date>',
			by: '<UserReference>'
		}
	}
}
```
With this splitting we use two reserved words; **on** and **set**. Other reserved property names:

```js
let notAllowed = ['on','get', 'set', 'init', 'emit', '_events', 'db', 'isNew', 'errors', 'schema', 'options', 'modelName', 'collection', '_pres', '_posts', 'toObject']
let notAllowedWithAlternatives = {
	'on': ['moment'],
	'emit': [],
	'_events': [],
	'db': [],
	'get': ['receive'],
	'set': ['put'],
	'init': [],
	'isNew': [],
	'errors': [],
	'schema': [],
	'options': [],
	'modelName': [],
	'collection': [],
	'_pres': [],
	'_posts': [],
	'toObject': [],
}
```
*Note: we suggest avoiding these words even as a part of your property names since later splitting will cause problems (the example with setOn and setBy could be improved by using putMoment and putBy)*

Javascript JSON [asks](https://www.w3resource.com/slides/json-style-guide.php) you to refrain from using these at the root of your JSON Object: 
```js
kind, fields, etag, id, lang, updated, deleted, currentItemCount, itemsPerPage, startIndex, totalItems, pageIndex, totalPages, pageLinkTemplate, next, nextLink, previous, previousLink, self, selfLink, edit, editLink
```




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

