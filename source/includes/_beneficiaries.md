# Beneficiaries

Beneficiaries are used to make payments from one person to another person.
The recipient can have an account which is an onnet account (on Symelation) or an
offnet account (which is held at a South African Bank).

Symelation integrates into the South African National Payment System to provide
EFT Credit services to users to move money from their Diamond Cash Card account to other
bank accounts.

## List Beneficiaries for a User

```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/beneficiaries"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": [
    {
      "uuid": "0ef6c2ca-40c8-11e5-99a6-df67e1f2a887",
      "name": "Bob",
      "bank_id": "1",
      "branch_code": "000000",
      "account_number": "12345678910",
      "my_reference": "Bob",
      "their_reference": "Cliff"
    },
    {
      "uuid": "1ef6c2ca-40c8-11e5-99a6-df67e1f2a887",
      "name": "Ted",
      "bank_id": "11",
      "branch_code": "051001",
      "account_number": "063010585",
      "my_reference": "Ted",
      "their_reference": "Cliff"
    }
  ]
}
```

This endpoint lists beneficiaries belonging to a specific user.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users/<USER>/beneficiaries`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user the beneficiary belongs to

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the beneficiary
name      | string(32) | Users name for the recipient (i.e. Telkom - Home Phone)
bank_id   | integer | Bank Idenfier
branch_code | integer | Branch code for the bank account of the beneficiary - where possible please use the banks universal branch code
account_number | integer | Bank Account Number for the beneficiary typically 9 to 11 digits in length
reference1 | string(32) | Users reference for their account transaction list when making the payment
reference2 | string(32) | Reference displayed on the beneficiaries bank transaction list

## Create Beneficiary

```shell
curl -X POST -d '{"name":"Vendor Name","bank_id":"1","branch_code":"000000","account_number":"53220000024","account_type":"1","reference1":"Vendor Name","reference2":"My Bill ID"}' "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/beneficiaries"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "details": {
        "uuid": "80d907f3-4f6f-4443-bafc-d1165509da61"
    }
}
```

This endpoint creates the specified beneficiary for the specific user.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users/<UUID>`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user the beneficiary belongs to

### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
name      | string(32) | Users name for the recipient (i.e. Telkom - Home Phone)
bank_id   | integer | Bank Idenfier
branch_code | integer | Branch code for the bank account of the beneficiary - where possible please use the banks universal branch code
account_number | integer | Bank Account Number for the beneficiary typically 9 to 11 digits in length
reference1 | string(32) | Users reference for their account transaction list when making the payment
reference2 | string(32) | Reference displayed on the beneficiaries bank transaction list

## Delete Beneficiary

This endpoint tombstones the specific beneficiary belonging to a specific user.

```shell
curl -X DELETE "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/beneficiaries/2dfd6a70-c4b7-11e5-90ee-ddee8a7cd8b5"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

### HTTP Request

`DELETE https://127.0.0.1.xip.io/api/v1/users/<USER>/beneficiaries/<BENEFICIARY>`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user the beneficiary belongs to
BENEFICIARY | The UUID of the beneficiary being tombstoned

## Pay a Beneficiary

This endpoint creates a payment the specific beneficiary belonging to a specific user.

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/beneficiaries/bd9e473d-d8cf-4a30-b639-f2e21ee7a5ec"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"amount":"50000"}'
```
### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/beneficiaries/<BENEFICIARY>`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user the beneficiary belongs to
BENEFICIARY | The UUID of the beneficiary being paid

### POST Parameters

Parameter | Description
--------- | -----------
amount | Amount of money to transfer to the user in cents

The OTP is available via `GET https://127.0.0.1.xip.io/api/v1/users/<USER>/beneficiaries/<BENEFICIARY>/payments/<PAYMENT>/otp` in the
development environment to obtain the 6 character OTP code for posting to https://127.0.0.1.xip.io/api/v1/users/<USER>/beneficiaries/<BENEFICIARY>/payments/<PAYMENT>/release.

## Retreiving the OTP in Test

This endpoint retreives the OTP for releasing a payment being made to specific beneficiary belonging to a specific user.
Please note that this route is only exposed in our test environment.

```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/beneficiaries/bd9e473d-d8cf-4a30-b639-f2e21ee7a5ec/payments/40351988-d0d7-40a3-b558-92e35e2af8ac/otp"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/beneficiaries/<BENEFICIARY>/payments/<PAYMENT>/otp`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user the beneficiary belongs to
BENEFICIARY | The UUID of the beneficiary being paid
PAYMENT | The UUID of the queued to be processed payment for the beneficairy

## Release a payment (for the end-user confirming the OTP)

This endpoint releases the payment instruction to the specific beneficiary belonging to a specific user.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/beneficiaries/<BENEFICIARY>/payments/<PAYMENT>/release`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user the beneficiary belongs to
BENEFICIARY | The UUID of the beneficiary being paid
PAYMENT | The UUID of the queued to be processed payment for the beneficairy

### POST Parameters

Parameter | Description
--------- | -----------
otp | One Time Password to release the payment to the beneficiary

## List Banks (for use when creating a beneficiary)

```shell
curl "https://127.0.0.1.xip.io/api/v1/banks"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": [
    {
      "id": "1",
      "bank_name": "Diamond Cash Card",
      "default_branch_code": "000000"
    },
    {
      "id": "2",
      "bank_name": "ABSA",
      "default_branch_code": "632005"
    },
    {
      "id": "3",
      "bank_name": "African Bank",
      "default_branch_code": "430000"
    },
    ...
  ]
}
```

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/banks`

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
bank_name | string (64) | Name of the bank
default_branch_code | integer | The central or universal branch code for the given bank
