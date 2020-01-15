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
// Get About Info
accredo.About().then(function (res) {
    console.log(res);
});

// Get Company Info
accredo.Company().then(function (res) {
    console.log(res);
});

// Get Version Info
accredo.Version().then(function (res) {
    console.log(res);
});

// Get System Period
accredo.SystemPeriod().then(function (res) {
    console.log(res);
});

// Get System Date
accredo.SystemDate().then(function (res) {
    console.log(res);
});

// Get Current User
accredo.CurrentUser().then(function (res) {
    console.log(res);
});


// Query raw SQL

accredo.sql("SELECT FIRST 2 CustomerCode, CustomerName FROM ARCUST").then(function (res) {
    console.log(res);
    // Print: [ { CustomerCode: '0001', CustomerName: 'Cash Sales' }, { CustomerCode: '0003', CustomerName: 'ASB Bank Ltd' } ]
}); 

accredo.sql("SELECT FIRST 1 CustomerCode, CustomerName FROM ARCUST").then(function (res) {
    console.log(res);
    // Print: [{ CustomerCode: '0001', CustomerName: 'Cash Sales' }]
});

// Please use sqlRow if you only want the single row
accredo.sqlRow("SELECT FIRST 1 CustomerCode, CustomerName FROM ARCUST").then(function (res) {
    console.log(res);
    // Print: { CustomerCode: '0001', CustomerName: 'Cash Sales' }
});

accredo.sql("SELECT COUNT(*) FROM ARCUST").then(function (res) {
    console.log(res);
    // Print: [{ Count: 180 }]
});

// Please use sqlOne if you only want to get one field from the first row
accredo.sqlOne("SELECT COUNT(*) FROM ARCUST").then(function (res) {
    console.log(res);
    // Print:  180 
});
```


**OAuth2 Login Support**
```javascript

const accredoAPIUrl = 'http://localhost:6567/saturn/odata4/v1/company(\'demo\')/';
const accredoOptions = {
    token_url: 'http://localhost:6567/saturn/oauth2/v1/token',
    client_id : 'hbx1xYj886jy7vZ_',
    username: 'DEMO',
    password: 'DEMO',
    cache_dir: '/tmp',  // The temporary directory for storing token
}

// Initiate Accredo
const accredo = new Accredo(accredoAPIUrl, accredoOptions);
```


**Control Entity**
```javascript
async () => {
    const customer = new ARCustomer();
    customer.CustomerCode = 'TEST';
    customer.CustomerName = 'Test Name';
    await customer.save();
}
```
