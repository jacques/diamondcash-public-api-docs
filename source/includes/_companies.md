# Companies

## Get All Companies (Customers)

```shell
curl "https://127.0.0.1.xip.io/api/v1/companies"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": [
    {
      "uuid": "9ee3314c-40dd-11e5-9bc4-1bbf7f69525d",
      "name": "Bob the builder"
    }
    ...
  ]
}
```

This endpoint retrieves all companies which can be paginated over via the API.  Typically for agencies this is just your agency account.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/companies`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Specifies the maximum number of companies to fetch
offset | 0 | Starts from the first user up to limit companies

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the company

## Create payments for release

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/companies/UUID/payments"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '[{"branch_code":"00000000","account_number":"12345678901","reference1":"Payment to Tarsai","reference2":"Payment from Mandla Enterprises","amount":"123425"},{"branch_code":"00000000","account_number":"12345678901","reference1":"Payment to Ayibongwe","reference2":"Payment from Mandla Enterprises","amount":"123425"}]'
```

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":{
    "batch_uuid":"2b1c1f24-c5ff-11e5-99c3-5a8f9bcf73ec",
    "payments": "1",
    "amount": "10000"
  }
}
```

> JSON Schema for Payments

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "id": "http://jsonschema.net",
  "type": "array",
  "title": "Root schema.",
  "description": "Defines payments to be queued for release via a company authorised contact who has transact rights to the company account",
  "name": "/",
  "items": {
    "id": "http://jsonschema.net/0",
    "type": "object",
    "title": "0 schema.",
    "description": "Describes a single beneficiary payment",
    "name": "0",
    "properties": {
      "branch_code": {
        "id": "http://jsonschema.net/0/branch_code",
        "type": "string",
        "title": "Branch Code",
        "description": "Branch code for the bank account / wallet account.  Default to 000000 for an on Symelation transaction else use the universal branch code for the bank.",
        "name": "branch_code",
        "default": "00000000"
      },
      "account_type": {
        "id": "http://jsonschema.net/0/account_type",
        "type": "string",
        "title": "Bank Account Type",
        "description": "Bank account type. 1 == Current / Cheque Account | 2 == Savings account | 3 == Transmission account",
        "name": "account_type",
        "default": "1"
      },
      "account_number": {
        "id": "http://jsonschema.net/0/account_number",
        "type": "integer",
        "title": "Bank Account / Wallet Account Number",
        "description": "The bank account number or wallet account number for the recipient of the payment",
        "name": "account_number",
        "default": ""
      },
      "reference1": {
        "id": "http://jsonschema.net/0/reference1",
        "type": "string",
        "title": "Your reference",
        "description": "This is the reference that goes onto your business accounts statement",
        "name": "reference1",
        "default": ""
      },
      "reference2": {
        "id": "http://jsonschema.net/0/reference2",
        "type": "string",
        "title": "Beneficiary Reference",
        "description": "This is the reference that goes onto your business accounts statement",
        "name": "reference2",
        "default": ""
      },
      "amount": {
        "id": "http://jsonschema.net/0/amount",
        "type": "integer",
        "title": "Amount in ZAR cents",
        "description": "Amount of money to pay over to the recipient in ZAR cents",
        "name": "amount",
        "default": "",
        "minimum": 0
      }
    },
    "required": [
      "branch_code",
      "account_type",
      "account_number",
      "reference1",
      "reference2",
      "amount"
    ]
  },
  "required": [
    "0"
  ],
  "minItems": "1"
}
```

This endpoint is used to create payment batches for release on the Symelation Platform.  It supposed both On Net (i.e. to other users on the platform)
or Off Net (i.e. to an off platform bank account such as a vendors bank account at any bank).  The branch code 000000 indicates that the payment
is an On Net payment.  `account_type` should be 1 for an On Net transaction and `branch_code` 000000.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/companies/UUID/payments`

### URL Parameters

Parameter | Description
--------- | -----------
UUID | The UUID of the company to load payment instructions against

### JSON Payload Parameters

Uses an array of arrays of the following parameters.  Expects to find 1 or more arrays with the following keys:

Parameter | Type | Description
--------- | ---- | -----------
branch_code | string(6) | Branch code (i.e. 000000 for On Net, 580105 for Investec, etc.)
account_type | integer | Bank Account Type. 1 == Current / Cheque Account | 2 == Savings account | 3 == Transmission account.  Use 1 for On Net Payments.
account_number | integer | The bank account number or wallet account number for the recipient of the payment
reference1 | string(16) | This is the reference that goes onto your business accounts statement
reference2 | string(16) | This is the reference that goes onto your beneficiaries accounts statement
amount | integer | Amount of money to pay over to the recipient in ZAR cents

## Get Payment Batches

```shell
curl "https://127.0.0.1.xip.io/api/v1/companies/UUID/payments"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "details": {
        "batches": [
            {
                "uuid": "95e6caf4-eb8c-11e5-9feb-ab8eea7dff53",
                "payments": 8,
                "amount": 84000
            },
            {
                "uuid": "9623cef4-eb8c-11e5-b16b-4341e2905df5",
                "payments": 28,
                "amount": 294000
            },
            ...
        ]
    }
}
```

This endpoint retrieves all payments associated with a payment batch which can be paginated over via the API.  Typically for agencies this is just your agency account.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/companies/UUID/payments/BATCH_UUID`

### URL Parameters

Parameter  | Description
---------- | -----------
UUID       | The UUID of the company to retrieve

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Specifies the maximum number of companies to fetch
offset | 0 | Starts from the first user up to limit companies

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the company

## Get Payment Batch Status

```shell
curl "https://127.0.0.1.xip.io/api/v1/companies/UUID/payments/BATCH_UUID"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "details": {
        "batches": [
            {
                "uuid": "95e6caf4-eb8c-11e5-9feb-ab8eea7dff53",
                "payments": 8,
                "amount": 84000
            },
            {
                "uuid": "9623cef4-eb8c-11e5-b16b-4341e2905df5",
                "payments": 28,
                "amount": 294000
            },
            ...
        ]
    }
}
```

This endpoint retrieves all payments associated with a payment batch which can be paginated over via the API.  Typically for agencies this is just your agency account.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/companies/UUID/payments/BATCH_UUID`

### URL Parameters

Parameter  | Description
---------- | -----------
UUID       | The UUID of the company to retrieve
BATCH_UUID | The BATCH_UUID of the company to retrieve

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Specifies the maximum number of companies to fetch
offset | 0 | Starts from the first user up to limit companies

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the company
