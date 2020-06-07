#accredo 

"accredo" is a promise-based Node.js ORM for accessing Accredo through Accredo Web Service API. It features solid transaction support, read, write and more.

"accredo" follows SEMVER. Supports Node v8 and above to use ES6 features.

## Why use accredo?

Instead of putting lots of time on building yourself scaffolding of accessing Accredo V5, we have encapsulated all Accredo Web API fundamental functions into the library, just use it to access Accredo.

## Installation
```javascript
$npm install accredo
```
 
## Initiation

```javascript
// In ES5 
const Accredo = require('accredo').default;
const accredo = new Accredo("http://localhost:6567/saturn/odata4/v1/company('demo')/");

// In ES6
import Accredo from 'accredo';
const accredo = new Accredo("http://localhost:6567/saturn/odata4/v1/company('demo')/");

```

If you set OAuth2 login token required in Accredo Web API, you need to fill in token parameters when initiating:
```js
const accredoAPIUrl = 'http://localhost:6567/saturn/odata4/v1/company(\'demo\')/';
const accredoOptions = {
    token_url: 'http://localhost:6567/saturn/oauth2/v1/token',
    client_id : 's1h2u3i4w5a6n7g8_',
    username: 'DEMO',
    password: 'DEMO',
    cache_dir: '/tmp',  // The temporary directory for storing token data
};
const accredo = new Accredo(accredoAPIUrl, accredoOptions);
```

## Execute Raw SQL

You can access data by executing raw sql as same as SQL Query Builder in the Accredo client, you are unable to modify data in this mode.

- function sql()  // Get results, return an array or null
- function sqlRow()   // Get the first row of the result, return an object or null
- function sqlOne()   // Get the first column of the first row of the result, return a primitive value or null

E.g.:
```js

import Accredo from 'accredo';
const accredo = new Accredo("http://localhost:6567/saturn/odata4/v1/company('demo')/");

accredo.sql("SELECT FIRST 2 CustomerCode, CustomerName FROM ARCUST").then(function (res) {
    console.log(res);
    // Print: [ { CustomerCode: '0001', CustomerName: 'Cash Sales' }, { CustomerCode: '0003', CustomerName: 'ASB Bank Ltd' } ]
}); 

accredo.sql("SELECT FIRST 1 CustomerCode, CustomerName FROM ARCUST").then(function (res) {
    console.log(res);
    // Print: [{ CustomerCode: '0001', CustomerName: 'Cash Sales' }]
});

// Please use sqlRow if you only get the first row of the result
accredo.sqlRow("SELECT FIRST 2 CustomerCode, CustomerName FROM ARCUST").then(function (res) {
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
## Endpoint Access
You can define endpoints to CRUD them through accredo, please refer their data structure in the $metadata page before defining: http://localhost:6567/saturn/odata4/v1/Company('demo')/$metadata  

Let us use ARCustomer as an example:

#### 1. Get the ARCustomer's structure in the $metadata page:
```xml
<EntityType Name="ARCustomer" HasStream="False">
<Key>
<PropertyRef Name="CustomerCode"/>
</Key>
<Annotation Term="Accredo.AccredoType" String="MasterFile"/>
<Annotation Term="Accredo.Supports">
<Record>
<PropertyValue Property="Accredo.IsClientControlledID" Bool="True"/>
<PropertyValue Property="Accredo.CanInsert" Bool="True"/>
<PropertyValue Property="Accredo.CanDelete" Bool="True"/>
<PropertyValue Property="Accredo.AllowPut" Bool="True"/>
<PropertyValue Property="Accredo.AllowPatch" Bool="True"/>
<PropertyValue Property="Accredo.AllowPost" Bool="True"/>
<PropertyValue Property="Accredo.AllowPutInsert" Bool="True"/>
<PropertyValue Property="Accredo.AllowPostInsert" Bool="True"/>
</Record>
</Annotation>
<Property Name="RecNo" Type="Edm.Int32" Nullable="true"/>
<Property Name="CustomerCode" Type="Edm.String" Nullable="false"/>
<Property Name="CustomerName" Type="Edm.String" Nullable="true"/>
<Property Name="CustomerGroupCode" Type="Edm.String" Nullable="false"/>
<Property Name="Address1" Type="Edm.String" Nullable="true"/>
<Property Name="Address2" Type="Edm.String" Nullable="true"/>
<Property Name="Address3" Type="Edm.String" Nullable="true"/>
<Property Name="Address4" Type="Edm.String" Nullable="true"/>
<Property Name="Address5" Type="Edm.String" Nullable="true"/>
<Property Name="PostCode" Type="Edm.String" Nullable="true"/>
<Property Name="OtherPartyCode" Type="Edm.String" Nullable="true"/>
<Property Name="SalesPersonCode" Type="Edm.String" Nullable="false"/>
<Property Name="WebAddress" Type="Edm.String" Nullable="true"/>
<Property Name="BankReference" Type="Edm.String" Nullable="true"/>
<Property Name="BankParticulars" Type="Edm.String" Nullable="true"/>
<Property Name="PhoneNo" Type="Edm.String" Nullable="true"/>
<Property Name="PhoneNo2" Type="Edm.String" Nullable="true"/>
<Property Name="FaxNo" Type="Edm.String" Nullable="true"/>
<Property Name="StatementStyle" Type="Edm.String" Nullable="false"/>
<Property Name="SalesAreaCode" Type="Edm.String" Nullable="false"/>
<Property Name="PriceCode" Type="Edm.String" Nullable="false"/>
<Property Name="DiscountScheduleCode" Type="Edm.Int16" Nullable="true"/>
<Property Name="BillToAccountCode" Type="Edm.String" Nullable="true"/>
<Property Name="Comment" Type="Edm.String" Nullable="true"/>
<Property Name="PeriodOpeningBalance" Type="Edm.Double" Nullable="true"/>
<Property Name="YearOpeningBalance" Type="Edm.Double" Nullable="true"/>
<Property Name="Balance3" Type="Edm.Double" Nullable="true"/>
<Property Name="Balance2" Type="Edm.Double" Nullable="true"/>
<Property Name="Balance1" Type="Edm.Double" Nullable="true"/>
<Property Name="BalanceCurrent" Type="Edm.Double" Nullable="true"/>
<Property Name="BalanceFuture" Type="Edm.Double" Nullable="true"/>
<Property Name="BalanceTotal" Type="Edm.Double" Nullable="true"/>
<Property Name="CreditLimit" Type="Edm.Double" Nullable="true"/>
<Property Name="SalesPeriodToDate" Type="Edm.Double" Nullable="true"/>
<Property Name="SalesYearToDate" Type="Edm.Double" Nullable="true"/>
<Property Name="SalesLastYear" Type="Edm.Double" Nullable="true"/>
<Property Name="DateOfLastSale" Type="Edm.Date" Nullable="true"/>
<Property Name="AmountOfLastReceipt" Type="Edm.Double" Nullable="true"/>
<Property Name="DateOfLastReceipt" Type="Edm.Date" Nullable="true"/>
<Property Name="PayerName" Type="Edm.String" Nullable="true"/>
<Property Name="TaxNo" Type="Edm.String" Nullable="true"/>
<Property Name="BackOrder" Type="Edm.Boolean" Nullable="true"/>
<Property Name="StopCredit" Type="Edm.Boolean" Nullable="true"/>
<Property Name="OrderNoRequired" Type="Edm.Boolean" Nullable="true"/>
<Property Name="BranchCode" Type="Edm.String" Nullable="true"/>
<Property Name="LocationCode" Type="Edm.String" Nullable="true"/>
<Property Name="DefaultDeliveryCode" Type="Edm.String" Nullable="true"/>
<Property Name="BankThrough" Type="Edm.Boolean" Nullable="true"/>
<Property Name="MediaCode" Type="Edm.String" Nullable="false"/>
<Property Name="StatementEmail" Type="Edm.String" Nullable="true"/>
<Property Name="InvoiceEmail" Type="Edm.String" Nullable="true"/>
<Property Name="PackingSlipEmail" Type="Edm.String" Nullable="true"/>
<Property Name="QuoteEmail" Type="Edm.String" Nullable="true"/>
<Property Name="GstOverride" Type="Edm.String" Nullable="true"/>
<Property Name="PrimaryContactID" Type="Edm.Int32" Nullable="true"/>
<Property Name="EmailAddress" Type="Edm.String" Nullable="true"/>
<Property Name="BankAccountNo" Type="Edm.String" Nullable="true"/>
<Property Name="Inactive" Type="Edm.Boolean" Nullable="true"/>
<Property Name="LastHistoryPeriod" Type="Edm.Int16" Nullable="true"/>
<Property Name="CreatedUserCode" Type="Edm.String" Nullable="true"/>
<Property Name="CurrencyCode" Type="Edm.String" Nullable="false"/>
<Property Name="CreatedTime" Type="Edm.TimeOfDay" Nullable="true"/>
<Property Name="ModifiedUserCode" Type="Edm.String" Nullable="true"/>
<Property Name="ModifiedDate" Type="Edm.Date" Nullable="true"/>
<Property Name="ModifiedTime" Type="Edm.TimeOfDay" Nullable="true"/>
<Property Name="Category1" Type="Edm.String" Nullable="true"/>
<Property Name="Category2" Type="Edm.String" Nullable="true"/>
<Property Name="CreatedDate" Type="Edm.Date" Nullable="true"/>
<Property Name="CountryCode" Type="Edm.String" Nullable="true"/>
<Property Name="Balance3Bs" Type="Edm.Double" Nullable="true"/>
<Property Name="Balance2Bs" Type="Edm.Double" Nullable="true"/>
<Property Name="Balance1Bs" Type="Edm.Double" Nullable="true"/>
<Property Name="BalanceCurrentBs" Type="Edm.Double" Nullable="true"/>
<Property Name="BalanceFutureBs" Type="Edm.Double" Nullable="true"/>
<Property Name="BalanceTotalBs" Type="Edm.Double" Nullable="true"/>
<Property Name="YearOpeningBalanceBs" Type="Edm.Double" Nullable="true"/>
<Property Name="PeriodOpeningBalanceBs" Type="Edm.Double" Nullable="true"/>
<Property Name="SalesPeriodToDateBs" Type="Edm.Double" Nullable="true"/>
<Property Name="SalesYearToDateBs" Type="Edm.Double" Nullable="true"/>
<Property Name="SalesLastYearBs" Type="Edm.Double" Nullable="true"/>
<Property Name="AutoAllocate" Type="Edm.String" Nullable="false"/>
<Property Name="RegimeCode" Type="Edm.String" Nullable="false"/>
<Property Name="BusinessNo" Type="Edm.String" Nullable="true"/>
<Property Name="Latitude" Type="Edm.Double" Nullable="true"/>
<Property Name="Longitude" Type="Edm.Double" Nullable="true"/>
<Property Name="EditRevision" Type="Edm.Int32" Nullable="true"/>
<Property Name="RecordRevision" Type="Edm.Int32" Nullable="true"/>
<Property Name="BalanceTotalExclFuture" Type="Edm.Double" Nullable="true"/>
<Property Name="BalanceTotalExclFutureBs" Type="Edm.Double" Nullable="true"/>
<NavigationProperty Name="Contact" Type="Collection(AccredoWS.ARCustomer_Contact)" ContainsTarget="true"/>
<NavigationProperty Name="DeliveryAddress" Type="Collection(AccredoWS.ARCustomer_DeliveryAddress)" ContainsTarget="true"/>
<NavigationProperty Name="Link" Type="Collection(AccredoWS.ARCustomer_Link)" ContainsTarget="true"/>
<Property Name="AllowInactive" Type="Edm.Boolean" Nullable="true"/>
</EntityType>
```

#### 2. Define the ARCustomer endpoint

There are 3 paramesters in the field definition
```js
    CustomerCode : {
        type : Accredo.String,  // Required field - Field type, check "Supported Data Types" in the below
        primaryKey: true,       // Optional field - If Primary Key
        allowNull: ture,        // Optional field - Default is true, if you set false, then you have to set the value before posting into Accredo
    }
```

Define ARCustomer
```js
import Accredo from 'accredo';
const accredo = new Accredo("http://localhost:6567/saturn/odata4/v1/company('demo')/");
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
```

Supported Data Types:
- Accredo.String,
- Accredo.Byte,
- Accredo.Word,
- Accredo.Smallint,
- Accredo.Integer,
- Accredo.Float,
- Accredo.Boolean,
- Accredo.Date,
- Accredo.Time,
- Accredo.DateTime,


#### 3. CRUD the endpoint

Create new Customer
```js
// Please define the ARCustomer as above "2. Define the endpoint" before using it.
const customer = ARCustomer.build({
    CustomerCode: 'DEMO_CUSTOMER',
    CustomerName: 'Demo Customer'
});
customer.save().then(function(customer){
    console.log("Customer has been created.", customer.CustomerCode);
})
```

Get the demo customer
```js
ARCustomer.find("DEMO_CUSTOMER").then(function(customer){
    console.log("Got the customer:", customer, "plain object:", customer.toJSON());
})
```

Update the demo customer
```js
ARCustomer.find("DEMO_CUSTOMER").then(function(customer){
    customer.CustomerName = 'DEMO Customer 1';
    customer.save().then(function(customer){
        console.log("Customer has been updated.", customer.CustomerCode);
    });
});
```

Delete the demo customer
```js
ARCustomer.find("DEMO_CUSTOMER").then(function(customer){
    customer.delete().then(function(){
        console.log("Customer has been deleted.");
    });
});
```

#### Search with specific conditions in the ARCustomer endpoint

Use the findAll function with specific parameters to search specific data
```js
ARCustomer.findAll({
    attributes: ['CustomerCode', 'CustomerName'],   // Only get two columns in the result
    limit: 10,  // Get 10 customers,
    skip:  10,  // Start from the 11th line, mostly for pagination.
    sort: [['CustomerCode', 'desc']],   // Order by CustomerCode DESC
    expand: ['Contact'],    // Set Contact if you want the customer's contacts including in the returned data, be aware of it needs Endpoint's support, please check the related endpoint's structure
    where: {
        Comment: '',  // Search Customer without Comment, will demonstrate how to use as below
    }
});
```

#### 4. Search the endpoint

Supported Operators
- Op.eq
- Op.ne
- Op.gte
- Op.gt
- Op.lte
- Op.lt
- Op.like
- Op.and
- Op.or

Search the first code 'A' in the customers
```javascript
const Op = require('accredo').Op;
ARCustomer.find({
    where: {
        CustomerCode : {
            [Op.like] : 'A%'   
        } 
    }
});
```

Search the first code 'A' and BackOrder allowed in the customers
```javascript
const Op = require('accredo').Op;
ARCustomer.find({
    where: {
        [Op.and]: [
            { 
                CustomerCode : {
                    [Op.like] : 'A%'   
                }
            },
            { 
                BackOrder : true
            },
        
        ],     
    }
});
```

Get the customers modified after 2020-06-01
```javascript
const Op = require('accredo').Op;
ARCustomer.find({
    where: {
        ModifiedDate : {
            [Op.gte] : '2020-06-01'
        }
    }
});
```



## Get basic information

```js
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
accredo.sql("SELECT FIRST 1 * FROM ARCUST").then(function (res) {
    console.log(res);
});
```


## Helpful Tips


#### Examples of using endpoint

```javascript
const ARCustomer = accredo.define(
    'ARCustomer',
    {
        CustomerCode: {
            type: Accredo.String,
            primaryKey: true,
            allowNull: false,
        },
        CustomerName: {
            type: Accredo.String
        },
        CreatedDate: {
            type: Accredo.Date
        },
        CreatedTime: {
            type: Accredo.String
        },
        ModifiedDate: {
            type: Accredo.Date
        },
        ModifiedTime: {
            type: Accredo.String
        }

    }
);
```

**Get data through Entity**

```javascript
// Get the specific customer by Primary Key
ARCustomer.find('ASHENG').then(function (res) {
    console.log("Fetch successfully", res.CustomerCode);
});

// Search items which balance is larger than $1000
ARCustomer.find({
    where: {
        Balance1: {
            [Op.le] : 1000
        }
    }
}).then(function (res) {
    console.log("Fetch successfully", res.length);
});

// Search by condition 'like'
import {Op} from 'accredo';
ARCustomer.find({
    where: {
        CustomerCode: {
            [Op.like] : 'B%'
        }
    }
}).then(function (res) {
    console.log("Fetch successfully", res.length);
});

// Get one record by search
ARCustomer.findOne({
    where: {
        Balance1: {
            [Op.le] : 1000
        }
    }
}).then(function (res) {
    console.log("Fetch successfully", res.length);
});

// Get the latest created customer
ARCustomer.findOne({
    order: [ ['CreatedDate', 'DESC'] , ['CreatedTime', 'DESC'] ]
}).then(function (res) {
    console.log(res.CustomerCode);
});

// Get the first 3 customrs from the result
ARCustomer.find({
    limit: 3
}).then(function (res) {
    console.log("Fetch successfully", res.length);
});

```

**Write data through Entity**

```javascript
// Create new Customer
const customer = new ARCustomer();
customer.CustomerCode = 'T' + Date.now();
customer.CustomerName = 'Name ' + customer.CustomerCode;
customer.save().then(function() {
    console.log(customer.CustomerCode + ' => ' + customer.CreatedTime);
});

// Edit Customer
ARCustomer.find('ASHENG').then(function (customer) {
    customer.CustomerName = 'ASHENG New Ltd';
    const preModifiedTime = customer.ModifiedTime;
    customer.save().then(function(){
        console.log("Edit " + ( preModifiedTime !== customer.ModifiedTime ? 'successfully' : 'failed' ))
    })
});

// Delete Customer
ARCustomer.find('ASHENG').then(function (customer) {
    customer.delete().then(function(){
        console.log("Delete successfully");
    });
});
```

**Get and Write Entity with Navigations**

```javascript
// Get INInvoice with lines
INInvoice.findOne({
   order : ['DocumentID', 'DESC'],
   expand: ['Line']
}).then(function(invoice){
    console.log(invoice);
});

// Edit Invoice
INInvoice.findOne({
   order : ['DocumentID', 'DESC'],
   expand: ['Line']
}).then(function(invoice){

    // Edit Head Info
    invoice.Comment = 'Invoice edited';
    
    // Add new Line
    invoice.Line.push({
        ProductCode: 'BED',
        Comment: 'This is a test',
    });
    
    // Edit Line Info
    const line = invoice.Line[0];
    line.Comment = 'Invoice first line edited';
    
    // Remove the first Line
    invoice.Line.shift();
    
    invoice.save().then(function(){
        console.log("Edit successfully");
    });
});
```

#### Data Type Validator
After setting the specific type for the entity field, the lib will validate your data type when submitting data. 

**allowNull validation**

```javascript
const ARCustomer = accredo.define(
    'ARCustomer',
    {
        CustomerCode: {
            type: Accredo.String,
            primaryKey: true,
            allowNull: false,
        },
    }
);
const customer = new ARCustomer();
customer.save().then(function(customer){}).catch(function(error){
    console.log( error.message);
});
```

**Call Functions**

You can get the details of function name and parameters information from the xml page $metadata

Get Period For Date

```javascript
accredo.PeriodForDate('2018-05-01').then(function (period) {
    console.log(period);
})
````

**Call Actions**

You can get the details of action name and parameters information from the xml page $metadata

Post Inoivce

```javascript
accredo.INInvoicePost(441, false, false, false).then().catch(function (error) {
                console.log(error.message);
            })
```


Download Purchase Order pdf file
```javascript
const purchase_order = await accredo.POOrder.find(1);
const pdf_file_content = await purchase_order.Print();
```


##Accredo Map Generator

We also provide the tool accredo-map for easily to generate endpoints, functions, and actions automatically per your requirements.

- Execute the following command

```bash
npx accredo-map --url="http://localhost:6567/saturn/odata4/v1/company('demo')"
```

- After executing the above command, the generator will generate the related javascript files to "accredo-map/demo/*"
- Import it to the accredo instance in your code as below, then you should be able to call all entities, functions and actions

```javascript
const Accredo = require('accredo').default;
const accredo = new Accredo("http://localhost:6567/saturn/odata4/v1/company('demo')/");
require('./accredo-map/demo')(accredo);

// Start using at here:
accredo.ARCustomer.find("DEMO_CUSTOMER").then(function(customer){
    console.log("Got the customer:", customer, "plain object:", customer.toJSON());
});
```

For more information and commercial support, welcome to contact [admin@bizex.co.nz](admin@bizex.co.nz)
