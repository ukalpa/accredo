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
import Accredo from 'accredo';
// Initiate Accredo Instance
const accredo = new Accredo("http://localhost:6567/saturn/odata4/v1/company('demo')/");

// Define an entity
const ARCustomer = accredo.define('ARCustomer', {
    Name : {
        type : Accredo.String,
    },
    IndexName : {
        type : Accredo.String,
    },
    RecordMax : {
        type : Accredo.Integer,
    },
    RecNo : {
        type : Accredo.Integer,
    },
    RecordCount : {
        type : Accredo.Integer,
    },
    CustomerCode : {
        type : Accredo.String,
        primaryKey: true,
    },
    CustomerName : {
        type : Accredo.String,
    },
    CustomerGroupCode : {
        type : Accredo.String,
    },
    Address1 : {
        type : Accredo.String,
    },
    Address2 : {
        type : Accredo.String,
    },
    Address3 : {
        type : Accredo.String,
    },
    Address4 : {
        type : Accredo.String,
    },
    Address5 : {
        type : Accredo.String,
    },
    PostCode : {
        type : Accredo.String,
    },
    OtherPartyCode : {
        type : Accredo.String,
    },
    SalesPersonCode : {
        type : Accredo.String,
    },
    WebAddress : {
        type : Accredo.String,
    },
    BankReference : {
        type : Accredo.String,
    },
    BankParticulars : {
        type : Accredo.String,
    },
    PhoneNo : {
        type : Accredo.String,
    },
    PhoneNo2 : {
        type : Accredo.String,
    },
    FaxNo : {
        type : Accredo.String,
    },
    StatementStyle : {
        type : Accredo.String,
    },
    SalesAreaCode : {
        type : Accredo.String,
    },
    PriceCode : {
        type : Accredo.String,
    },
    DiscountScheduleCode : {
        type : Accredo.Integer,
    },
    BillToAccountCode : {
        type : Accredo.String,
    },
    Comment : {
        type : Accredo.String,
    },
    PeriodOpeningBalance : {
        type : Accredo.Float,
    },
    YearOpeningBalance : {
        type : Accredo.Float,
    },
    Balance3 : {
        type : Accredo.Float,
    },
    Balance2 : {
        type : Accredo.Float,
    },
    Balance1 : {
        type : Accredo.Float,
    },
    BalanceCurrent : {
        type : Accredo.Float,
    },
    BalanceFuture : {
        type : Accredo.Float,
    },
    BalanceTotal : {
        type : Accredo.Float,
    },
    CreditLimit : {
        type : Accredo.Float,
    },
    SalesPeriodToDate : {
        type : Accredo.Float,
    },
    SalesYearToDate : {
        type : Accredo.Float,
    },
    SalesLastYear : {
        type : Accredo.Float,
    },
    DateOfLastSale : {
        type : Accredo.Date,
    },
    AmountOfLastReceipt : {
        type : Accredo.Float,
    },
    DateOfLastReceipt : {
        type : Accredo.Date,
    },
    PayerName : {
        type : Accredo.String,
    },
    TaxNo : {
        type : Accredo.String,
    },
    BackOrder : {
        type : Accredo.Boolean,
    },
    StopCredit : {
        type : Accredo.Boolean,
    },
    OrderNoRequired : {
        type : Accredo.Boolean,
    },
    BranchCode : {
        type : Accredo.String,
    },
    LocationCode : {
        type : Accredo.String,
    },
    DefaultDeliveryCode : {
        type : Accredo.String,
    },
    BankThrough : {
        type : Accredo.Boolean,
    },
    MediaCode : {
        type : Accredo.String,
    },
    StatementEmail : {
        type : Accredo.String,
    },
    InvoiceEmail : {
        type : Accredo.String,
    },
    PackingSlipEmail : {
        type : Accredo.String,
    },
    QuoteEmail : {
        type : Accredo.String,
    },
    GstOverride : {
        type : Accredo.String,
    },
    PrimaryContactID : {
        type : Accredo.Integer,
    },
    EmailAddress : {
        type : Accredo.String,
    },
    BankAccountNo : {
        type : Accredo.String,
    },
    Inactive : {
        type : Accredo.Boolean,
    },
    LastHistoryPeriod : {
        type : Accredo.Integer,
    },
    CreatedUserCode : {
        type : Accredo.String,
    },
    CurrencyCode : {
        type : Accredo.String,
    },
    CreatedTime : {
        type : Accredo.Time,
    },
    ModifiedUserCode : {
        type : Accredo.String,
    },
    ModifiedDate : {
        type : Accredo.Date,
    },
    ModifiedTime : {
        type : Accredo.Time,
    },
    Category1 : {
        type : Accredo.String,
    },
    Category2 : {
        type : Accredo.String,
    },
    CreatedDate : {
        type : Accredo.Date,
    },
    CountryCode : {
        type : Accredo.String,
    },
    Balance3Bs : {
        type : Accredo.Float,
    },
    Balance2Bs : {
        type : Accredo.Float,
    },
    Balance1Bs : {
        type : Accredo.Float,
    },
    BalanceCurrentBs : {
        type : Accredo.Float,
    },
    BalanceFutureBs : {
        type : Accredo.Float,
    },
    BalanceTotalBs : {
        type : Accredo.Float,
    },
    YearOpeningBalanceBs : {
        type : Accredo.Float,
    },
    PeriodOpeningBalanceBs : {
        type : Accredo.Float,
    },
    SalesPeriodToDateBs : {
        type : Accredo.Float,
    },
    SalesYearToDateBs : {
        type : Accredo.Float,
    },
    SalesLastYearBs : {
        type : Accredo.Float,
    },
    AutoAllocate : {
        type : Accredo.String,
    },
    RegimeCode : {
        type : Accredo.String,
    },
    BusinessNo : {
        type : Accredo.String,
    },
    Latitude : {
        type : Accredo.Float,
    },
    Longitude : {
        type : Accredo.Float,
    },
    EditRevision : {
        type : Accredo.Integer,
    },
    RecordRevision : {
        type : Accredo.Integer,
    },
    BalanceTotalExclFuture : {
        type : Accredo.Float,
    },
    BalanceTotalExclFutureBs : {
        type : Accredo.Float,
    },
    AllowInactive : {
        type : Accredo.Boolean,
    },
}, {navigations:[{"Name":"Contact","Type":"Collection(AccredoWS.ARCustomer_Contact)","ContainsTarget":"true"},{"Name":"DeliveryAddress","Type":"Collection(AccredoWS.ARCustomer_DeliveryAddress)","ContainsTarget":"true"},{"Name":"Link","Type":"Collection(AccredoWS.ARCustomer_Link)","ContainsTarget":"true"}]});


async () => {
    // Fetch data from the entity
    let customer = await ARCustomer.find({
        limit: 1,
        expand: ['Contact'] // Add this if you want to get customer contacts
    });
    console.log(customer.toJSON());
    
    // Edit existing customer
    customer.Description = customer.Description + ' test';
    await customer.save();

    // Insert data into ARCustomer
    const new_customer = new ARCustomer();
    new_customer.CustomerCode = 'TEST';
    new_customer.CustomerName = 'Test Name';
    await new_customer.save();
}
```
