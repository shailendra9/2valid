validate-me
=========

JavaScript data structure validator


## Installation
npm install validate-me --save


## Usage

```javascript
vm = require( 'validate-me' );

vm.registerModel( "user", {
  id:   { type: "uuid", required: true },        // property “id” required and must be uuid
  name: { type: "string", min: 4, max: 128 },    // property “name” must be String and contain 4-128
} );

vm.showModelsExpanded();

var valid = vm.validate( "user", {
      id    : "61cecfb4-da43-4b65-aaa0-f1c3be81ec53",
      name  : "Alex Bardanov",
    }) 

vm.consoleTrueOrError ( valid );
```

## Tests

  npm test


## Module description
### Methods and properties of myLibrary
-  registerModel( modelName, modelObject ) - register model modelName with modelObject
-  registeredModels - list of registered models
-  validate( modelName, entity ) - validate model modelName with entity
-  showModels( full ) - show information of registered models
-  showModelsFull() - show full information of registered model
-  dispose() - remove all registered modelNames
-  errors - list of errors
"Types" properties located in types.js and included description to validate of each type 
( "min" and "max" properties, add "check" method to check validation ).


## Add type
If you need add new type, you can do it in Types variable in types.js. Be shure to add "check" method 
to check new inserted type. Also you can add "min" and "max" properties to check length.

For example, new type "password". It type must contains minimum 4 chars: at least one lower 
and one upper case, digit and special chars.
Add code below to types.js in list property:
```javascript
password : {
    min   : 4,         // string.min Minimum length of the string
    max   : Infinity,  // string.max Maximum length of the string
    check : function( password ){   // check password type and size
        if ( ( typeof string === 'string' || string instanceof String )
                && string.length >= this.min 
                && string.length <= this.max 
                && string.match(/((?=.*\\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[\W]).+)/) 
        ) 
            return true
        else 
            return false;
    },
},
```

## Register model
For register model you need to use registerModel method.

```javascript
myLibrary.registerModel( "user_password", {
  id:   { type: "uuid", required: true },     // property “id” must be uuid
    // property “name” must be String and contain 4-128
  name: { type: "string", min: 4, max: 128, required: true }, 
    // property “password” must be String and contain 4-128 chars: 
    // at least one lower and one upper case, digit and special chars.
  password: { type: "password", max: 128, required: true },       
} );
```


## Validate object  
If you want validate new object, then insert before "myLibrary.dispose();" line:
```javascript
myLibrary.consoleTrueOrError ( myLibrary.validate( "user", "entity" ) );

For example, validate based on "user" model:
myLibrary.consoleTrueOrError ( 
    myLibrary.validate( "user", { id : "61cecfb4-da33-4b15-aa10-f1c6be81ec53", name : "Dimitry Ivanov", password : "A1z!" }) 
);
```


## Exceptions
- Name is undefined
```javascript
myLibrary.registerModel( "Name", { id: { type: "uuid", required: true } } );
```

- Model in "modelName" is undefined
```javascript
myLibrary.registerModel( "modelName", NaN );
```

- Model "modelName" is already registered
```javascript
myLibrary.registerModel( "modelName", { id: { type: "uuid", min: 1, max: 5, required: true } } );
myLibrary.registerModel( "modelName", { id: { type: "name" } } );
```

- No field "name" in key "name" in model "modelName"
```javascript
myLibrary.consoleTrueOrError ( myLibrary.validate( "modelName", { name  : "Alex Bardanov" }) );
```

- No type field exception
```javascript
myLibrary.registerModel( "name_exception", { date: { parameter: "date" } } );
```

- No guid type exception
```javascript
myLibrary.registerModel( "name_exception", { id: { type: "guid" } } );
```


## Errors
- Required field "key" not found in model "modelName"
- Field "key" not found in model "modelName"
- Field "key" not matched with type "type" in model "modelName"
- 2 errors: Field not matched with type exception and Field not found
```javascript
myLibrary.consoleTrueOrError ( myLibrary.validate( "user", { id : "1cecfb4-da43-4b65-aaa0-f1c3be81ec53", imya : "Alex Bardanov" }) );
```
- Size minimum check error
```javascript
myLibrary.consoleTrueOrError ( myLibrary.validate( "user", { id : "61cecfb4-da43-4b65-aaa0-f1c3be81ec53", name : "" }) );
```
- Size maximum check error
```javascript
myLibrary.consoleTrueOrError ( myLibrary.validate( "user", { id : "61cecfb4-da43-4b65-aaa0-f1c3be81ec53", name : "ASNKJW oew  owek rewRWIWJG OERGMLkf gsojejrwoeg ke r gerEGIOJWgij i4 ggr" }) );
```


## Release History

* 0.1.0 Initial release
* 0.2.0 Fix nested required object error
* 0.2.2 Update errors handles
* 0.2.4 Add match ability to string
* 0.2.10 Split number to integer and float, add password type


## To-Do
* Return wishes to validate ( ex. "Password must contain 4 digits and 4 most popular actor's names" )
* Try to find a hint to validate ( ex. "do you watn to find size, not a 'XXze'?" )


## Created by

Dimitry, 2@ivanoff.org.ua

curl -A cv ivanoff.org.ua

