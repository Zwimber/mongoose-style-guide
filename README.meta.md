### Creating reusable meta structure

To keep code dry it is usefull to think about what unites your models. We mention returning patterns shortly. 
In this chapter we will discuss the creation of a `meta` pattern. A definition of `meta` in the technical sense:

Technical metadata properties include file types, size, creation date and time, and type of compression. Technical metadata is often used for digital object management and interoperability.
[https://www.lifewire.com/metadata-definition-and-examples-1019177]

We aim to store the following fields in our meta object:

a) Created
b) Updated
c) Deleted
d) Restored

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

(+) Implementation [exists](https://www.npmjs.com/package/mongoose-delete)
(-) Hard to leave out or select meta, causes large selects
(-) We cannot see a full history

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

 - (+) Easy to leave out meta by default, it is rarely relevant
 - (-) We cannot see a full history
 - (-) Implementation does not exist


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

 - (+) Easy to leave out meta by default, it is rarely relevant 
 - (+) We can see a full history
 - (-) If a object is updated very often this array will grow very large . 


