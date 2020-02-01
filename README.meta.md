### Creating reusable meta structure

To keep code dry it is usefull to think about what unites your models. We mention returning patterns shortly. 
In this chapter we will discuss the creation of a `meta` pattern. A definition of `meta` in the technical sense:

> Technical metadata properties include file types, size, creation date and time, and type of compression.
> Technical metadata is often used for digital object management and interoperability.

Source: [lifewire.com](https://www.lifewire.com/metadata-definition-and-examples-1019177)

We aim to store the following fields in our meta object:

1. Created
2. Updated
3. Deleted
4. Restored

Obviously we are going for a `soft-delete` pattern. A hard delete will not leave any record in the database. 

#### Goals 

1. Make it the default to only query fields that are not `deleted`.
2. Make it eas to sort by `newest` versus `oldest` and `recently edited`
3. If you have a system with users; log which user did create, update or restore.


#### V0 - root

```
const eventSchema = new Schema({
    // ...
    createdAt: Date,
    createdBy: ObjectId,

    updatedAt: Date,
    updatedBy: ObjectId,

    deletedAt: Date,
    deletedBy: ObjectId,
})
```

 - (+) Implementation - [exists](https://www.npmjs.com/package/mongoose-delete) therefore easy
 - (-) Mongoose select - hard to leave out or select meta, causes large selects
 - (-) Blame - we cannot see a full history
 - (+) Size - very small and no risk of growing too large

#### V1 - meta

```
const eventSchema = new Schema({
    // ...
    meta: {
        createdAt: Date,
        createdBy: ObjectId,

        updatedAt: Date,
        updatedBy: ObjectId,

        deletedAt: Date,
        deletedBy: ObjectId,
    }
})
```

 - (-) Implementation - does not exist so has to be created by hand
 - (+) Mongoose select - easy to leave out meta by default and select when needed
 - (-) Blame - we cannot see a full history
 - (+) Size - very small and no risk of growing too large

#### V2 - meta + history

```
const eventSchema = new Schema({
    // ...
    meta: {
        created: { type: Boolean, default: true },
        updated: { type: Boolean, default: true },
        deleted: { type: Boolean, default: false },
        history: [{
            state: String,
            put: {
                at: Date,
                by: ObjectId,
            }
        }]
    }
})
```

 - (-) Implementation - does not exist so has to be created by hand
 - (+) Mongoose select - easy to leave out meta by default and select when needed
 - (+) Blame - we can see a full history
 - (-) Size - can grow to be a large part of size
 
