
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
* [Storing history]('#history')

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
**User** - capital since it is an class
**user** - lowecase, this will be the collection name in MongoDB, some mongoose versions will pluralize this


	// Generated icons
	icon_32 : SchemaImage,
	icon_64 : SchemaImage,
	icon_128: SchemaImage,
	icon_256: SchemaImage,
	icon_512: SchemaImage,



	start: SchemaExactmoment,
	startMargin: SchemaDuration,
	startCertainty: {
		type: String,
		enum: [
			'certain',	// Customer knows exactly when to move
			'flexible', // Customer knows when to move, but is flexible
			'estimate', // Customer gave an estimation of when to move
			'unknown',	// Customer does not know when to move
		],
	},
 --> mooi woord voor exact moment waarop iets gebeurt
 --> stamp
 --> datetime
 --> moment
 
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


### Property naming

For property names always use camelCase. Try to order parts of the word from more to lesser important. If we want to have a property that stores data of an profile picture we suggest naming that property: "pictureProfile". 

This might seem counterintuitive but this standardised way of property naming has several advantages:

 1. Easy refactoring to new namespace
 2. Readable
 3. Consistent, therefore easy to guess variable names

TLDR; tips when naming properties:

1. Use camelCase
2. Order camelCase parts from most to least important 
 (*do:* nameFirst, *don't*: firstName)
3. Don't restate the current model name
4. Be descriptive, MongoDB favours short variable names but unless you are an developer at Facebook don't bother too much.
5. Watch for reserved words

*Note: After this example we suggest an alternative way for storing username*
```js
// Good examples
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

// Bad examples
let user = {
	name_first: 'Tim',						// (1) Not using camelCase
	middleName: null,						// (2) Wrong order of elements 
	userNameLast: 'L',						// (3) Restated name of model 
	e: 'user@example.com',					// (4) Not descriptive
	options: {},							// (5) Using reserved property names
	
	picture_profile: '/url/to/profile.jpg',	// (1) Not using camelCase
	banner_picture: '/url/to/profile.jpg',	// (2) Wrong order of elements
	userActive: true, 						// (3) Restated name of model
}
```
### CamelCase versus object
Often you are confronted with a tradeof between camelcasing versus objectification. Consider the following:

```js
let user = {
    nameFirst: '',
    nameMiddle: '',
    nameLast: '',
}
```

```js
let user = {
    name: {
        first: '', 
        middle: '',
        last: '',
    }
}
```

The first second seems more verbose however it gives us several advantages:

 - Clear grouping of properties (shorter select objects)
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
*Note: in this example it is very unlikely you would not want to use any of the intermediate properties*

**Exception 1 - Whenever there is great advantage in using the root property**


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
on, emit, _events, db, get, set, init, isNew, errors, schema, options, modelName, collection, _pres, _posts, toObject
```
*Note: we suggest avoiding these words even as a part of your property names since later splitting will cause problems (the example with setOn and setBy could be improved by using putMoment and putBy)*


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

