# Wallets

Diamond Cash Wallets (accounts) provide a virtual account for depositing of money into one of the Symelation deposit accounts.

The Symelation system uses the following references for allocating money which has been deposited into a users wallet:

Reference|Description
----------|----------
Account Number|Wallet Account Number (i.e. goes into the specified wallet of the user)
South African ID Number|South African issued Identity Document from Home Affairs
MSISDN|MSISDN of the users Mobile Phone Number
Passport Number|Foreigners passport issued by a foreign country
Asylum Document Number|Asylum document issued by Home Affairs to a foreigner

When a user has multiple wallets, the first wallet that was created for the user
would receive the money when the wallets account number is not used in the deposit.

## Listing wallets for a user

```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/accounts"
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

`GET https://127.0.0.1.xip.io/api/v1/users/<USER>/accounts`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user to retrieve

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the wallet
account_number | integer | Account number for the wallet
description | string (64) | Description of the wallet (i.e Firstname's Wallet)
account_type | enum | `wallet` for wallet
balance | integer | balance of the wallet in cents

## Fetching a mini statement for a users wallet


```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/accounts/ecb23a8c-99dd-11e4-912d-ef2aab9046d8/ministatement"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": [
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
```

This endpoint retrieves a mini statement for the specific users wallet.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users/<UUID>/accounts/<ACCOUNT>/ministatement`

### URL Parameters

Parameter | Description
--------- | -----------
UUID | The UUID of the user to retrieve
ACCOUNT | The UUID of the account to retrieve

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the user

## Listing transactions for a users wallet

```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/accounts/ecb23a8c-99dd-11e4-912d-ef2aab9046d8/ministatement"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": [
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
```

This endpoint retrieves a specific user.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users/<UUID>/accounts/<ACCOUNT>/transactions`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Specifies the maximum number of users to fetch
offset | 0 | Starts from the first user up to limit users

### URL Parameters

Parameter | Description
--------- | -----------
UUID | The UUID of the user to retrieve
ACCOUNT | The UUID of the account to retrieve

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the user
