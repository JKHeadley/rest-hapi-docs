---
id: metadata
title: Metadata
sidebar_label: Metadata
---

## Timestamps
rest-hapi supports the following optional timestamp metadata:
- createdAt (default enabled, activated via `config.enableCreatedAt`)
- updatedAt (default enabled, activated via `config.enableUpdatedAt`)
- deletedAt (default enabled, activated via `config.enableDeletedAt`) (see [Soft delete](#soft-delete))

When enabled, these properties will automatically be populated during CRUD operations. For example, say I create a user with a payload of:

```json
 {
    "email": "test@email.com",
    "password": "1234"
 }
```

If I then query for this document I might get:

```json
 {
    "_id": "588077dfe8b75a830dc53e8b",
    "email": "test@email.com",
    "createdAt": "2017-01-19T08:25:03.577Z"
 }
```

If I later update that user's email then an additional query might return:

```json
 {
    "_id": "588077dfe8b75a830dc53e8b",
    "email": "test2@email.com",
    "createdAt": "2017-01-19T08:25:03.577Z",
    "updatedAt": "2017-01-19T08:30:46.676Z"
 }
```

The ``deletedAt`` property marks when a document was [soft deleted](#soft-delete).

**NOTE**: Timestamp metadata properties are only set/updated if the document is created/modified using rest-hapi endpoints/methods.
Ex: 

``mongoose.model('user').findByIdAndUpdate(_id, payload)`` will not modify ``updatedAt`` whereas

``restHapi.update(mongoose.model('user'), _id, payload)`` will. (see [Mongoose wrapper methods](#mongoose-wrapper-methods))

## User tags
In addition to timestamps, the following user tag metadata can be added to a document:
- createdBy (default disabled, activated via `config.enableCreatedBy`)
- updatedBy (default disabled, activated via `config.enableUpdatedBy`)
- deletedBy (default disabled, activated via `config.enableDeletedBy`) (see [Soft delete](#soft-delete))

If enabled, these properties will record the `_id` of the user performing the corresponding action. 

This assumes that your authentication credentials (request.auth.credentials) will contain either a `user` object with a `_id` property, or the user's \_id stored in a property defined by `config.userIdKey`.

**NOTE**: Unlike timestamp metadata, user tag properties are only set/updated if the document is created/modified using rest-hapi endpoints, (not rest-hapi [methods](#mongoose-wrapper-methods)).
