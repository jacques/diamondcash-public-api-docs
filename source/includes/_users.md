# Users

## Get All Users (Customers)

```shell
curl "https://127.0.0.1.xip.io/api/v1/users"
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
      "title": "MR",
      "first_name": "CLIFTON MARIO",
      "last_name": "HERADIEN",
      "id_type": "ZAID",
      "id_document_number": "12345678901",
      "mobile_number": "271234567890"
    },
    {
      "uuid": "9df55274-40dd-11e5-be41-033d23a48a0e",
      "title": "MR",
      "first_name": "TIM",
      "last_name": "COLMAN",
      "id_type": "ZAID",
      "id_document_number": "12345678901",
      "mobile_number": "271234567891"
    },
    ...
  ]
}
```

This endpoint retrieves all users which can be paginated over via the API.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Specifies the maximum number of users to fetch
offset | 0 | Starts from the first user up to limit users

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the user
title | string (4) | Title of the user (i.e. Mr / Mrs / Ms / Dr / Prof)
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)
id_type | enum | Identification Document Type (ZAID == South African ID / PASSPORT = Passport / ZAASYLUM = South African Asylum Document)
id_document_number | string (32) | Document number associated with the supplied Identification Document Type (i.e. for ZAID you supply the South African ID number / PASSPORT you supply the passport number)

## Get a Specific User

```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "uuid": "9ee3314c-40dd-11e5-9bc4-1bbf7f69525d",
    "username": "cliff12345",
    "title": "MR",
    "first_name": "CLIFTON MARIO",
    "last_name": "HERADIEN",
    "date_of_birth": "1970-01-01",
    "gender": "m",
    "mobile_number": "271234567890",
    "autoload_type": "percentage",
    "autoload_value": "0",
    "fica_status": "2",
    "tandc_accepted_at": "2015-02-15 21:54:01"
  }
}
```

This endpoint retrieves a specific user.  Additional information about users is available through Basecamp.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users/<USER>`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user to retrieve

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the user
title | string (4) | Title of the user (i.e. MR / MRS / MS / DR / PROF)
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)
date_of_birth | date | Date of birth of the user
gender | string (1) | Gender of the user (i.e. f == female | m == male)
mobile_number | integer | MSISDN for the user in E.164 format (minus the leading +)
autoload_type | enum | Autoload type to use when deposits are made into the user's wallet (percentage == a percentage gets autoloaded | monetary == a set amount in cents gets sent over when the value exceeds the value set)
autoload_value | integer | When autoload_type == percentage in a range of 0 to 100.  When autoload_type == monetary the amount > the value will trigger the transaction to autoload the value to the card)
fica_status | integer | The FICA status for the user (0 == No FICA document supplied / 1 == South African ID / Passport / Asylum Document provided and KYC complaince offer has approved the document | 2 == Proof of address has been provided for the user in addition to the users proof of identity)
tandc_accepted_at | date time | The date the user accepted the terms and conditions

## Create a customer

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/users"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -d '{"title":"MRS","first_name":"CHERYL","last_name":"SERFONTEIN","date_of_birth":"1970-01-01","gender":"f","id_type":"ZAID","id_document_number":"7001010101081","address1_type":"work","address1_street1":"7th Floor, West Tower, Canal Walk Shopping Center","address1_street2":"Century City Boulevard, Century City","address1_suburb":"Milnerton","address1_city":"Cape Town","address1_province":"ZA-WC","address1_postalcode":"7490","address1_country":"ZA",mobile_number":"08012345679"}'
```

> The above command returns JSON structured like this:

```json
{
  "status":"ok",
  "details":{
    "uuid":"2b1c1f24-c5ff-11e5-99c3-5a8f9bcf73ec",
    "account_number":"53224172946"
  }
}
```
This endpoint creates a new customer on the Symelation Platform.  During the process a wallet is created for the customer.
If autoload settings are not specified, the defaults listed below are utilised.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users`


### JSON Payload Parameters

Parameter | Type | Description
--------- | ---- | -----------
title | string (4) | Title of the user (i.e. MR / MRS / MS / DR / PROF)
first_name | string (64) | First names of the user
last_name | string (64) | Surname of the user
date_of_birth | date | Date of birth of the user
gender | string (1) | Gender of the user (i.e. f == female / m == male)
id_type | string | Identification Document Type (ZAID == South African ID / PASSPORT = Passport / ZAASYLUM = South African Asylum Document)
id_document_number | string (32) | The document number for the given identification type
passport_expiration_date | date | Date of expiration of the passport **REQUIRED IF id_type IS PASSPORT**
passport_country | string (2) | [ISO 3166 Alpha 2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for the country **REQUIRED IF id_type IS PASSPORT**
asylum_expiration_date | date | Date of expiration of the South African Asylum Document **REQUIRED IF id_type IS ZAASYLUM**
address1_type | enum | **home** for home address / **work** for employers address
address1_street1 | string (64) | Street Address
address1_street2 | string (64) | Street Address cont. **OPTIONAL**
address1_suburb | string (64) | Suburb
address1_city | string (64) | Name of the City or Town
address1_province | string (6) | [ISO 3166-2 ZA code](https://en.wikipedia.org/wiki/ISO_3166-2:ZA)
address1_postalcode | string (4) | Postal Codes for South Africa
address1_country | string (2) | [ISO 3166 Alpha 2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for the country
mobile_number | integer | MSISDN for the user in E.164 format (minus the leading +)
home_phone_number | integer | Phone number for the user in E.164 format (minus the leading +) **OPTIONAL**
email_address | string(64) | Email address of the user
autoload_type | enum | percentage or monetary for performing an autoload to a users Diamond Cash Card Master Card.
autoload_value | integer | When percentage enter a percentage from 0 to 100 for 0% or 100% for example.  When monetary, it allows specifying an amount in cents to autoload i.e. 200000 for R 2000.00)
key_field | varchar(32) | Your key field for the user (i.e. user id on your system)

### Defaults

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
autoload_type | enum | percentage
autoload_value | integer | 0 | Can be changed by the user once they have been allocated a pre-paid debit card
assign_to_agent | string(36) | null | Specifies which agent should be assigned to the user being created

## Delete a customer

```shell
curl -X DELETE "https://127.0.0.1.xip.io/api/v1/users/934abcb1-ab94-4bbf-9435-b8ac15e825e2"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

```json
{
  "status": "ok",
  "message": "User tombstoned."
}
```

This endpoint tombstones a specific user and cancels any products and debit cards a customer has.

### HTTP Request

`DELETE https://127.0.0.1.xip.io/api/v1/users/<USER>`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user to tombstone

## Resend Welcome SMS

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/users/5370faea-ec41-11e5-b754-13ac8769ead7/resendwelcomesms"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

### HTTP REQUEST

`POST /api/v1/users/<USER>/resendwelcomesms`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user to resend the SMS message to

## Search Users (Customers)

```shell
curl "https://127.0.0.1.xip.io/api/v1/users?first_name=Clifton"
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
      "title": "MR",
      "first_name": "CLIFTON MARIO",
      "last_name": "HERADIEN",
      "id_type": "ZAID",
      "id_document_number": "12345678901",
      "mobile_number": "271234567890"
    },
    ...
  ]
}
```

This endpoint retrieves all users which can be paginated over via the API.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit | 100 | Specifies the maximum number of users to fetch
offset | 0 | Starts from the first user up to limit users
first_name | | If specified searches for the users first name
last_name | | If specified searches for the users surname
id_type | ZAID | Identification Document Type (ZAID == South African ID / PASSPORT = Passport / ZAASYLUM = South African Asylum Document)
id_document_number | | Searches for the given government ID
mobile_number | | If specified searches for the given mobile number

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the user
title | string (4) | Title of the user (i.e. Mr / Mrs / Ms / Dr / Prof)
first_name | string (64) | First names of the user (needs to be in CAPITALS for the juristic person)
last_name | string (64) | Surname of the user (needs to be in CAPITALS for the juristic person)
id_type | enum | Identification Document Type (ZAID == South African ID / PASSPORT = Passport / ZAASYLUM = South African Asylum Document)
id_document_number | string (32) | Document number associated with the supplied Identification Document Type (i.e. for ZAID you supply the South African ID number / PASSPORT you supply the passport number)
mobile_number | integer | MSISDN for the user in E.164 format (minus the leading +)
