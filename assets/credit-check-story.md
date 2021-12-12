## Credit Check Story at BigCo, Inc.

### Purpose
To run a simple credit check periodically on companies that are our customers so that we can update their bsaseline spending limit
and discount percentages based on their credibilities

### Data
Each company record has one or more `account` records. 
Each `account` record as a `spendingLimit` property and a `discountPercentage` property. 
By default these two values are set to `5000` and `5%`, respectively with a default rating (??)
However, we can modify those values by running a simple credit check using the customer name to see if we need to adjust those values up or down.

We have an internal account history that is used to generate suggested ratings for companies we deal with. 
The details on how we generate these numbers is kept secret. 

### Processing
The credit check service has a safe method (`CreditCheckForm`) which accepts a `companyName` 
and returns a `CreditCheckItem` response which contains the property `rating` (a single value between 1 and 10). 

That `rating` value can then be used to determine BigCo's standard `spendingLimit` and `discountPercentage` for that company's account record. 

Each time the credit-check service is queried, it also writes a history record (`CreditCheckItem`) that can be called up later for review. 
The action `CreditCheckHistory` is recalled using the same `companyName` that was used when making the initial credit check. 
History records contain the following properties: `identifier`, `companyName`, `dateRequested`, and `rating`.

We run this credit-check when we first add a company account record and once a year after that (always on January 1st).  