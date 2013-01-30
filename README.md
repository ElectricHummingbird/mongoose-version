# Passport-Local Mongoose
Mongoose plugin to save document data versions. Documents are saved to a "versioned" document collection before saving
original documents and kept for later use.

## Installation

    $ npm install mongoose-version

## Usage
To use mongoose-version for an existing mongoose schema you'll have to require and plugin mongoose-version into the 
existing schema.

The following schema definition defines a "Page" schema, and uses mongoose-version plugin with default options

    var mongoose = require('mongoose'),
        Schema = mongoose.Schema,
        version = require('mongoose-version');
    
    var Page = new Schema({
        title : { type : String, required : true},
        content : { type : String, required : true },
        path : { type : String, required : true},
        tags : [String],
    
        lastModified : Date,
        created : Date
    });
    
    Page.plugin(version);

Mongoose-version will define a schema that is equivalent to the Page schema but adds two fields

* refId Id of the referenced Page model
* refVersion Version of the referenced Page model

Mongoose-version will add a static field to Page, that is "VersionedModel" that can be used to access the versioned
model of page, for example for querying old versions of a document.

*Attention* mongoose-version will increment the original mongoose model version using the increment method. 
Read [this blog post](http://aaronheckmann.blogspot.co.at/2012/06/mongoose-v3-part-1-versioning.html) for more details.

## Option keys and defaults
* collection: name of the collection to persist versions to. The default is 'versions'. You should supply this option if you're using mongoose-version on more than one schema.
* logError: specifies if a console.log message should be written when the versioned model could not be persisted. Default `false`
* surpressVersionIncrement: mongoose-version will increment the version of the saved model before saving the model. To turn of auto version increment set this option to true. Default: `false`
* strategy: mongoose-version allows versioned document to be saved as multiple documents in a collection or in a single document in a version array. In case you want to save documents in an array specify `array` strategy, for storing versioned documents in multiple documents specify `collection` strategy. Default `collection`.
* maxVersions: Only valid for `array` strategy. Specifies how many historic versions of a document should be kept. Defaults to `Number.MAX_VALUE`.
* Options are passed to the newly created mongoose as settings, so you may use any [option supported by mongoose](http://mongoosejs.com/docs/guide.html#options) 

In case you only want to specify the collection name, you can pass a string instance to options that is taken as collection name.