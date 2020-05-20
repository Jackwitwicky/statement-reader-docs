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
                "description_metadata": "254712456765 FTC191106MNAF",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            },
            {
                "id": 55,
                "amount": 20000.0,
                "description": " m-Banking Transfer",
                "transaction_type": "debit",
                "remaining_balance": 10000.84,
                "description_metadata": "254712456765 FTC191106MNAF",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            }
        ]
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
                "description_metadata": "254712456765 FTC191106MNAF",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            },
            {
                "id": 55,
                "amount": 20000.0,
                "description": " m-Banking Transfer",
                "transaction_type": "debit",
                "remaining_balance": 10000.84,
                "description_metadata": "254712456765 FTC191106MNAF",
                "transacted_at": "2019-11-06T00:00:00.000Z"
            }
        ]
    }
```

This endpoint retrieves a specific Analysis Result.

### HTTP Request

`GET localhost:3000/api/v1/fetch_analysis/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Statement Summary. (Obtained when you submitted statement for analysis)


