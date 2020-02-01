
# Node.js Mongoose Style Guide

This is a guide for writing consistent and aesthetically pleasing mongoose models.
It is inspired by what is popular within the community, and flavored with some
personal opinions.

The style-guide style is largely based on [this style-guide](https://github.com/felixge/node-style-guide) and assumes knowledge of it.

This guide was created by [Woodland](https://woodl.nl/) and is
licensed under the [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/)
license. You are encouraged to fork this repository and make adjustments
according to your preferences.

![Creative Commons License](https://i.creativecommons.org/l/by-sa/3.0/88x31.png)

## Table of contents

### Mongoose basics

* [Standards](#standards)
* [Folder structure](#folder-structure)
* [Schema structure](#schema-structure)
* [Schema grouping](#schema-grouping)
* [Motivation for singular form](#motivation-for-singular-form)

### Mongoose flat versus structured

* [Tradeof: Flat versus structured](#camelcase-versus-object)

### Mongoose population

* [Populateable guide](#populate)

### Mongoose stable patterns
To be added, we will discuss some often repeating patterns and how to name these
* [Storing history](#history)
* [Storing history](#history)

* Always store _id when storing history, this way you can revert to a certain stage. 


## Mongoose basics

### Standards


* Use American spelling


### Folder structure

Make sure to have a seperate folder for most *Mongoose* or *MongoDB* related.

1. Root file where you combine all models and export them ([example](#schema-grouping))
2. Group files related to a model together, use singular form
3. Always use index.js for the root schema ([example](#basics))
4. Create a shared folder which contains reusable schemas

```bash
|-- models
    |-- index.js         # (1)
    |-- user             # (2)
        |-- index.js     # (3) 
        |-- email.js
    |-- company
    |-- product
    |-- shared           # (4)
        |-- count.js
        |-- name.js
        |-- amount.js
        |-- duration.js

```

### Schema structure

Basic structure of an exported schema. Avoid specifying more than one schema per file. 

```js
// (0) Requires
let Schema = require('mongoose').Schema
let SchemaObjectId = Schema.Types.ObjectId;

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

### Schema grouping

Your schemas are grouped in another file. You can require these models in another folder but since in a lot of projects communication with the database is so commonplace that we suggest storing them in a global variable. 

As you can see in the following example, we leave the Schema, folder and model property names in singular form a motivation is found in the next chapter.

```js 
let model = require('mongoose').model

let SchemaUser = require(root + '/path/to/models/user/')
let SchemaProduct = require(root + '/path/to/models/product/')
let SchemaCompany = require(root + '/path/to/models/company/')

module.exports = {
    User: model('user', SchemaUser),
    Company: model('company', SchemaCompany),
    Product: model('product', SchemaProduct),
}
```

**User** is a class so it is UpperCamelCased

**user** is a collection name in MongoDB which by convention are lowercase

### Motivation for singular form

I am not saying you should ban plural from your programming alltogether, there are 

0. It does not give extra insight. 

```js
<Here will an extremely clear example showing that it does not give extra insight>
```
1. The English language has [rather confusing plurals](https://en.wikipedia.org/wiki/English_plurals) 
2. Plural (often) makes the word longer

| Singular          | Plural            | Character gain  |   Comment               |
|:------------------|:------------------| ---------------:|-------------------------|
| User              | Users             | +25%            |                         |
| Life              | Lives             | +25%            |                         |
| Dish              | Dishes            | +50%            |                         |
| Mouse             | Mice              | -20%            | Shorter!                |
| Radius            | Radii             | -17%            | Shorter!                |
| Staff             | Staffs            | +20%            |                         |
| Staff             | Staves            | +20%            | Alternative plural      |
| Child             | Children          | 60%             |                         |
| Bison             | Bison             | +0%             |                         |
| Company           | Companies         | +29%            |                         |
| Product           | Products          | +14%            |                         |
| Statistics        | Statistics        | +0%             | Plural singular         |

3. Object creation is slightly less readable in plural form

```js
let model = require(root + '/path/to/models/')

// Singular - good boy example
let user = new model.User()
let userQuery = model.User.find()

// Plural - bad boy example
let user = new model.Users()
let userQuery = model.Users.find()
```

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
4. Be descriptive, even though MongoDB favours short property names
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
    name_first: 'Tim',                      // (1) Not using camelCase
    middleName: null,                       // (2) Wrong order of elements 
    userNameLast: 'L',                      // (3) Restated name of model 
    e: 'user@example.com',                  // (4) Not descriptive
    options: {},                            // (5) Using reserved property names
    
    picture_profile: '/url/to/profile.jpg', // (1) Not using camelCase
    banner_picture: '/url/to/profile.jpg',  // (2) Wrong order of elements
    userActive: true,                       // (3) Restated name of model
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
*Note: in this example it is very unlikely you would not want to use any of the intermediate properties (e.g. we might a place to store the picture path `age.verification.picture.path`)*

**Exception 1 - Intermediate property makes no sense**

In cases where you want to be very descriptive and are not interested in using the intermediate fields using camelCase can be useful.

```js
// Before 
let user = {
    livingroomTelevisionCount: 1,
    twoPersonSofaCount: 1,
}

// After de-camelCase-ization
let user = {
    living: {
        room: {
            television: {
                count: 1
            }
        }
    },
    two: { person: { Sofa: { count: 1 } } }
}
```

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
    'on': ['moment', 'at'],
    'emit': [],
    '_events': [],
    'db': [],
    'get': ['receive'],
    'set': ['put', 'made'],
    'init': ['create'],
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
*Note: we suggest avoiding these words even as a part of your property names since later splitting will cause problems (the example with setOn and setBy could be improved by using putAt and putBy)*

Javascript JSON [asks](https://www.w3resource.com/slides/json-style-guide.php) you to refrain from using these at the root of your JSON Object: 
```js
kind, fields, etag, id, lang, updated, deleted, currentItemCount, itemsPerPage, startIndex, totalItems, pageIndex, totalPages, pageLinkTemplate, next, nextLink, previous, previousLink, self, selfLink, edit, editLink
```


### Flat vs structured

```js
// Please add me
```

### Populateable guide

Mongoose offers a very powerful function namely `populate`. It enables you to easily find linked models. Consider an typical N:N example with `users` that can do `transactions`. We'd easily want to do the following:

1. Add a transaction
2. Find all transactions that belong to a user
3. Find all users belonging to a transaction

Therefore a user and a transaction might have the following structure

```js
// User
let user = {
	_id: ObjectId,
	name: String,
	transaction: [{ 
		type: ObjectId, 
		ref: 'transaction' 
	}]
}

let transaction = {
	_id: ObjectId,
	amount: Number,
	user: [{ 
		type: ObjectId, 
		ref: 'user' 
	}]
}
```

This would enable us to do the following:

```js
let find = {}
let populate = { path: 'transaction' }
let query = Model.User.find(find).populate(populate)

// Result is a single object which combines transactions and users:
{
	_id: 0,
	name: 'Bob',
	transaction: [{
		_id: 1000,
		amount: 5,
		users: [0, 1]
	}, {
		_id: 1001
		amount: 6,
		users: [0, 3]
	}]
}
```

Without the use of population we need to do the following steps for the same result:

1. Find users
2. Make a list of all their transaction ids
3. Find all those transactions
4. Combine the user object with the found transactions

Population does come with a downside: there is uncertainty in the object structure. In situations where you do not populate the object structure is different. People also might forget that it is in fact a populated field:

```js
// Result when not populated
{
	_id: 0,
	name: 'Bob',
	transaction: [1000,1001] // List of transaction _id's
}

// Result when populated
{
	_id: 10,
	name 'Bob',
	transaction: [{ _id: 1000, amount: 5 }, { _id: 1001, amount: 6 }]
}
```
This is a clear source of errors, if at a certain time we decide that a specific route will have the populated version all old uses of that route that need the transaction _id will break.

We therefore suggest the following pattern whenever you make references:

```js
// V0
let SchemaTransactionRef = new Schema({
	item: { type: ObjectId, ref: 'transaction' },
	itemId: { type: ObjectId },
})

// V1
let SchemaTransactionRef = new Schema({
	transaction: { type: ObjectId, ref: 'transaction' },
	transactionId: { type: ObjectId },
})

// User
let SchemaUser = new Schema({
	_id: ObjectId,
	name: String,
	transaction: [SchemaTransactionRef]
})
```

This way we can safely use the identifier without the risk of it being populated:

- V0 -`transactionRef[0].itemId`
- V1 -`transactionRef[0].transactionId`

Please note that V1 might seem a little verbose but especially in situations like the following it is usefull:

```js

// 0 
// - Makes clear that it is a reference
user.transactionRef.forEach(ref => {
	const transaction = ref.item
	const transactionId = ref.itemId
})

// 1
// - In shorthand loops it is clear 
user.transactionRef.forEach(ref => {
	const transaction = ref.transaction
	const transactionId = ref.transactionId
})
user.transactionRef.map(ref => ref.transaction.price)

// 2
user.transactionRef.forEach(transactionRef => {
	const transaction = transactionRef.item
	const transactionId = transactionRef.itemId
})

// 3
// user.transactionRef.forEach(item => {
// 	const transaction = item.item
// 	const transactionId = item.itemId
// })

```

### Path/route guide

In many modern webapplications your backend 

/:idUser/update



workorderRef: [{
	item: 
	itemId:
}]

workorder: [{
	workorderId: 
	workorder: 
	....
}]

## API Response style

The API should embrace a small set of statuscodes. It is cumbersome to check these and some might trigger behaviour of the client (often a browser). Therefore we've chosen to only use the following:

**5XX ERROR** connection failures

**404 NOT FOUND** failure, used for routes that do not exist

**401 UNAUTHORIZED** not logged in

**200 SUCCESS** for all other requests since the request completed succesfully

In a functioning application, only the 200 & 500 statuscode is expected. All the other codes are a sign of a failing application. 

### Reponse data

Whenever a statuscode 200 is read this does not mean the request is succesful. We only know the real status after parsing it's content JSON. 

The state of a response is specified by it's properties. The properties *data* and *warning* are used to confirm succesful actions. The properties *error* and *failure* convey problems after which you shouldn't continue. 

The message added is only for the programmer. It is the clients responsibility to create a proper message. 

### Situational examples

**failures**
 - Cannot connect to database
 - Object 'user' does not exist
 - Property X does not exist on Y
 - This _id is not unique (when unexpected)

**errors**
 - Password is incorrect

**warning**
 - Request took 5 seconds

**success**
 - We found this user
 - We found these events

**cannot read property x of y** response with failure. This should never happen, application should break down.
**incorrect password** error 

### Response example

```javascript
const response = {

    // Optional extra information for developer
    message: String,
    
    // Success, continue
    data: [] || {
		// warning?
	}, 

    // Error, do not continue 
    // (e.g. insufficient funds)
    error: {

		// Used to differentiate between errors
		reason: 'insufficientFunds',
		
		name: '',			// DEV => Err.name
		code: 400,			// DEV => 
		stack: '',			// DEV => Err.stack
	},

    // Extra information about request
    meta: {}
}
```


GET `/event/:id/guest`
POST `/event/`
PATCH `/event/:id/guest/:guestId/arrived`
REMOVE `/event/:id/guest`

AUTH = post 

transactionService() {

}

transactionService.get() 



transaction = {
	get: 
	remove: 
}

transactionGet() {

}
transactionremove() {

}



## Where to do what?

Where does which format get relevant?
