#Accredo 

Accredo is a promise-based Node.js ORM for accessing Accredo through Accredo Web Service API. It features solid transaction support, read, write and more.

Accredo follows SEMVER. Supports Node v8 and above to use ES6 features. Instead of putting lots of time on building yourself scaffolding of accessing Accredo 5, we have encapsulated all fundamental entities, functions and actions from Accredo Web API into the library.  

_For more information and commercial support, welcome to contact admin@bizex.co.nz_

## Installation
```bash
$ npm install --save accredo
```
 
## Usage

**Initiation**
```javascript
// ES5 
const Accredo = require('accredo').default;

// ES6
import Accredo from 'accredo';

// Initiate Accredo Instance
const accredo = new Accredo("http://localhost:6567/saturn/odata4/v1/company('demo')/");

```

**Basic Information**
```javascript
// Get Company Info
accredo.Company.then(function (res) {
    console.log(res);
});

// Get Version Info
accredo.Version.then(function (res) {
    console.log(res);
});

// Get System Period
accredo.SystemPeriod.then(function (res) {
    console.log(res);
});

// Get System Date
accredo.SystemDate.then(function (res) {
    console.log(res);
});

// Get Current User
accredo.CurrentUser.then(function (res) {
    console.log(res);
});

// Query raw SQL
accredo.sql("SELECT FIRST 1 * FROM ARCUST").then(function (res) {
    console.log(res);
});;
```

**Control Entity**
```javascript
const customer = new ARCustomer();
customer.CustomerCode = 'TEST';
customer.CustomerName = 'Test Name';
await customer.save();
```
