---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Statement Reader API! You can use our API to access Statement Reader endpoints, which can help you analyse a user's bank statement and return json data ready for further analysis.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

The API is grouped into three key sections

- Analysis Request --  This is used to submit a bank statement for analysis and parsing, together with getting the full analytical data for an already existing analysis
- Statement Summary - Used to get key summary for an analysed bank statement.
- Statement Transaction - You can use this to get details of a particular transaction from a bank statement

# Authentication

> Authorization of API is still under development. In the meantime, you can send requests as we test out the core functionality of the API. Please use this power responsibly.

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Statement Reader uses API keys to allow access to the API. You can register a new Statenent Reader API key at our [developer portal](http://example.com/developers).

Statement Reader expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Supported Banks

This is a list of Kenyan bank statements our platform can successfully analyse. Together with the proper key to use 
when submitting analysis requests

Bank Name | Bank Key | Status
--------- | ------- | -----------
NCBA | ncba | available
KCB | kcb | in-development
E-quity | equity | in-development
NCBA Loop | loop | in-development
Co-Operative Bank | coop | in-development
National Bank | national | in-development
Family Bank | family | in-development


# Analysis Request

## Analyse Statement

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "localhost:3000/api/v1/analysis_request"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
    "id": "3a57d3c9-745c-4cd9-8bc4-ea3ed0fc5e54",
    "bank_name": "ncba",
    "status": "pending",
    "created_at": "2020-05-21T15:11:54.105Z",
    "statement_summary": {
        "id": "29f7926d-1c7b-4a7b-b5c2-d1d22b1d6cf8",
        "opening_balance": 13075.84,
        "payments_in": 0.0,
        "payments_out": 2075.0,
        "available_balance": 110000.84,
        "closing_balance": 10000.84,
        "began_at": "2019-11-01T00:00:00.000Z",
        "closed_at": "2019-11-30T00:00:00.000Z",
        "statement_transactions": [
            {
                "id": 54,
                "amount": 75.0,
                "description": " Transaction Charge",
                "transaction_type": "debit",
                "remaining_balance": 13000.84,
                "description_metadata": "254712456765 FTC209806BHSD",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            },
            {
                "id": 55,
                "amount": 20000.0,
                "description": " m-Banking Transfer",
                "transaction_type": "debit",
                "remaining_balance": 10000.84,
                "description_metadata": "254712456765 FTC209806BHSD",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            }
        ]
    }
}
```

This endpoint submits a bank statement to obtain analysis data

### HTTP Request

`POST localhost:3000/api/v1/analysis_request`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
bank_name | ncba | This is the string denoting the bank this statement is for
statement_url | nil | This is the path to the uploaded pdf bank statement you would like to analyse
statement_password  | nil | Contains the password to unlock the statement. Can be blank if statement does not need a password

<aside class="success">
Remember — In future this endpoint will need to be Authenticated
</aside>

## Get Analysis Status

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "localhost:3000/api/v1/analysis_requests/<ID>"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
    "analysis_request": {
        "id": "03652226-a163-4177-8087-6ca9cfff7725",
        "bank_name": "ncba",
        "status": "pending",
        "created_at": "2020-05-21T10:42:28.365Z"
    }
}
```

This endpoint retrieves a specific Analysis Request. It can be used to check the status of the request allowing you to only fetch the full analysis request details when the status is ```success```. It will also show if the submitted request failed

### HTTP Request

`GET localhost:3000/api/v1/analysis_requests/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Analysis Request. (Obtained when you submitted statement for analysis)


## Get a Specific Analysis Result

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "localhost:3000/api/v1/fetch_analysis/statement_id"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
    "id": "3a57d3c9-745c-4cd9-8bc4-ea3ed0fc5e54",
    "bank_name": "ncba",
    "status": "pending",
    "created_at": "2020-05-21T15:11:54.105Z",
    "statement_summary": {
        "id": "29f7926d-1c7b-4a7b-b5c2-d1d22b1d6cf8",
        "opening_balance": 13075.84,
        "payments_in": 0.0,
        "payments_out": 2075.0,
        "available_balance": 110000.84,
        "closing_balance": 10000.84,
        "began_at": "2019-11-01T00:00:00.000Z",
        "closed_at": "2019-11-30T00:00:00.000Z",
        "statement_transactions": [
            {
                "id": 54,
                "amount": 75.0,
                "description": " Transaction Charge",
                "transaction_type": "debit",
                "remaining_balance": 13000.84,
                "description_metadata": "254712456765 FTC209806BHSD",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            },
            {
                "id": 55,
                "amount": 20000.0,
                "description": " m-Banking Transfer",
                "transaction_type": "debit",
                "remaining_balance": 10000.84,
                "description_metadata": "254712456765 FTC209806BHSD",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            }
        ]
    }
}
```

This endpoint retrieves a specific Analysis Result.

### HTTP Request

`GET localhost:3000/api/v1/fetch_analysis/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Analysis Request. (Obtained when you submitted statement for analysis)

# Statement Summary

## Get Statement Summary

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "localhost:3000/api/v1/statement_summaries/<ID>"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
    "statement_summary": {
        "id": "59281c69-59cc-49cf-aa3c-3173b7423552",
        "opening_balance": 12000.84,
        "payments_in": 0.0,
        "payments_out": 5000.0,
        "available_balance": 7000.84,
        "closing_balance": 7000.84,
        "began_at": "2020-01-01T00:00:00.000Z",
        "closed_at": "2020-01-31T00:00:00.000Z"
    }
}
```

This endpoint queries for an existing statement summary for an already analysed bank statement. The summary holds key details such as total money in the customer's account, how much money has gone in and out of the account and a time period of when the data is accounted for.

### HTTP Request

`GET localhost:3000/api/v1/statement_summaries/<ID>`

### Query Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Statement Summary. (Obtained when you submitted bank statement for analysis)


# Statement Transaction

## Get Statement Transaction

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "localhost:3000/api/v1/statement_summaries/<ID>"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
{
    "statement_transaction": {
        "id": "03b466d3-1139-4da0-b047-5d3509b1d1c9",
        "amount": 30.0,
        "description": " ATM Charges FTC2545JNSHF",
        "transaction_type": "debit",
        "remaining_balance": 19070.84,
        "description_metadata": "NCBA Wabera ATM2 Nairobi KE 425199......9046",
        "transacted_at": "2020-01-03T00:00:00.000Z"
    }
}
```

This endpoint queries for an existing statement transcation for an already analysed bank statement.

### HTTP Request

`GET localhost:3000/api/v1/statement_summaries/<ID>`

### Query Parameters

Parameter | Description
--------- | -----------
ID | The ID of the specific Statement Transaction. (Obtained when you submitted bank statement for analysis)


