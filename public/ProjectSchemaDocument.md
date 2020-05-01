# Understand Backengine App/ Schema/ Document

To build and manage data with backengine. You need to understand some of the concepts that backengine uses.

## Backengine App

Every application used Backengine API needs a corresponding Backengine App created on the Backengine dashboard.

Application connecting to backengine via API should provide backengine api a valid `App Secret` of the corresponding Backengine App.

## Schema

The schema is like a document describe how you store a specific data type. It also includes other special descriptions so that Backengine can understand and process your data.

**There are 3 types of schema:**

#### Normal Schema

This is the default schema type, it does not limit data access and editing rights. It requires providing the app's `App Secret` properly. 

It is popular with public data that does not belong to the user, and anyone can access and edit it.

#### Restrict Schema

This type of schema describes data that cannot be edited by the user via the API. It can only be edited directly on the Backengine dashboard.

It is suitable for fixed data such as level data, or configs.

#### Auth Schema

This is a special type of schema that allows you to define how the application authenticates users.

You can authenticate your end users the way you want by defining a schema of this type.

Each application has only one `Auth Schema`.

#### Relation Schema

This type of schema can only be created when there is at least one `Auth Schema`

The data in this schema will always be associated with a `Document` of `Auth Schema`

All the data that end users create in the `Relation Schema` will be refer to a `Document` in the `Auth Schema`.

This is exactly how Backengine authenticates users. A user needs to provide a correct `Document` in the `Auth Schema`, then that user has rights to access to the data refer to that `Document`.


## Document

A `Document` is a data record stored as described by a `Schema`. It is like a row in the data table.

All application data will be stored as documents. Based on the `Schema`s you have defined before. Backengine will know how to store and process these data the way you want.
