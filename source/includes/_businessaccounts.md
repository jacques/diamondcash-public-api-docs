# Business Accounts

Diamond Cash Business Accounts provide a virtual account for depositing of money into one of the Symelation deposit accounts.

The Symelation system uses the following references for allocating money which has been deposited into a company's business account:

Reference|Description
---------|----------
Account Number|Wallet Account Number (i.e. goes into the specified business account of the company)

## Listing business accounts for a company

```shell
curl "https://127.0.0.1.xip.io/api/v1/companies/d19bff36-4733-11e5-946b-9ba904d8238e/accounts"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": [
    {
      "uuid": "ecb23a8c-99dd-11e4-912d-ef2aab9046d8",
      "account_number": "44445555666",
      "description": "Tim's Wallet",
      "account_type": "wallet",
      "balance": "123456"
    }
  ]
}
```

This endpoint retrieves a collection of wallets belonging to the specific user.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/companies/<USER>/accounts`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user to retrieve

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the wallet
account_number | integer | Account number for the wallet
description | string (64) | Description of the wallet (i.e Company Name's Wallet)
account_type | enum | `business` for a business account
balance | integer | balance of the business account in cents

## Listing transactions for a company's business account

```shell
curl "https://127.0.0.1.xip.io/api/v1/companies/d19bff36-4733-11e5-946b-9ba904d8238e/accounts/ecb23a8c-99dd-11e4-912d-ef2aab9046d8/transactions"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "account": {
      "uuid": "ecb23a8c-99dd-11e4-912d-ef2aab9046d8",
      "account_number": "44445555666",
      "description": "Tim's Wallet",
      "account_type": "wallet",
      "balance": "123456"
    },
    "transactions": [
      {
        "txn_ref": "alc80075",
        "statement_date": "2015-02-19",
        "reference1": "EFT IN FEE",
        "reference2": "REF: 0801234567",
        "amount": "-90"
      },
      {
        "txn_ref": "61hk1xyo",
        "statement_date": "2015-02-19",
        "reference1": "CREDIT TRANSFER",
        "reference2": "0801234567",
        "amount": "50000"
      },
      ...
    ]
  }
}
```

This endpoint retrieves a specific business accounts transactions.  Without specifying any pagination
options using the Query Parameters specified below, you get a maximum  of 100 transactions and you can
then paginate over the resultset to retreive rows 101 to 200 then rows 201 to 300 etc.  The timespan
is the last 60 days on the account (presently disabled timespan for testing purposes for onboarding).

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/companies/<UUID>/accounts/<ACCOUNTUUID>/transactions`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Specifies the maximum number of users to fetch
offset | 0 | Starts from the first user up to limit users

### URL Parameters

Parameter | Description
--------- | -----------
UUID | The UUID of the company to retrieve
ACCOUNTUUID | The UUID of the account associated with the company listed above

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
txn_ref | string(8) | Transaction reference
statement_date | date | Date of a transaction on an account (note deposits display the date they appear on the bank statment from Standard Bank the associated transactional fees are reflected on the date they occurred on the account)
reference1 | string(32) | Transaction Type Narrative
reference2 | string(64) | Description Narrative
amount | integer | Amount in ZAR cents
