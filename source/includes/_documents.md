# Documents

As part of the regulatory requirements, documentation is collected from 
customers as part of the process of KYC / FICA.

## List Documents for a User

```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/documents"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": [
    {
      "uuid": "895b2fcc-545f-11e7-8279-67635886d357",
      "document_type": 1,
      "file_name": "55af7356-2641-11e6-9641-51a94b3cc203 - POID - GOVID - Vin Diesel.pdf",
      "file_hash": "4f9ef9c744b05f339b4d74c83a5e98c361ab8a7c35f95532e0ec977e71bfd321",
      "file_size": 3718903,
      "created_at": "2017-06-01 15:33:20"
    },
    {
      "uuid": "8a9e349c-545f-11e7-9a2b-273aed7d1a54",
      "document_type": 2,
      "file_name": "16517ff6-c74f-11e6-ab92-1708c4dea62c - POA - GOVID - Vin Diesel.pdf",
      "file_hash": "78d6405718fda873f99a60b722138451c136576c9f665139e0de0d78cc4be6ff",
      "file_size": 1640282,
      "created_at": "2017-06-01 15:33:45"
    }
  ]
}
```

This endpoint retrieves a collection of debit cards belonging to the specific user.

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users/<USER>/documents`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user whose list of uploaded documents you would like to retrieve


### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the document
document_type | integer | Indicates the type of document.  1 == Proof of Identification (South African ID / Passport / South African Asylum Document). 2 == Proof of address.  3 == Proof of Income.  4 == Application Form.  5 == Proof of Employment.  6 == Selfie. 7 == Affidavit. 9 == Home Visit. 10 == Proof of Payment (i.e. deposit slip / atm slip / payslip / etc.)
file_name | string (255) | File name including a random uuid at the front of the file name
file_hash | string (255) | sha256 of the uploaded file
file_size | integer | Size of the uploaded file
created_at | datetime | Datetime in GMT that the file was uploaded

## Upload a Document

```shell
curl -X POST "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/documents"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
  -F=@FILE.pdf
```

> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "details": {
    "uuid": "895b2fcc-545f-11e7-8279-67635886d357",
    "document_type": 1,
    "file_name": "55af7356-2641-11e6-9641-51a94b3cc203 - POID - GOVID - Vin Diesel.pdf",
    "file_hash": "4f9ef9c744b05f339b4d74c83a5e98c361ab8a7c35f95532e0ec977e71bfd321",
    "file_size": 3718903,
    "created_at": "2017-06-01 15:33:20"
  }
}
```

This endpoint allows for uploading of documents from a remote system.  The encoding type
needs to be multipart/form-data.  The file needs to be the equivalent of a input type of
file with a name of file:

```html
<form method="POST" enctype="multipart/form-data">
   <input type="file" name="file">
</form>
```

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/documents`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user who you want to upload a document for

### Response Result Set

Parameter | Type | Description
--------- | ---- | -----------
uuid | string (36) | UUID of the document
document_type | integer | Indicates the type of document.  1 == Proof of Identification (South African ID / Passport / South African Asylum Document). 2 == Proof of address.  3 == Proof of Income.  4 == Application Form.  5 == Proof of Employment.  6 == Selfie. 7 == Affidavit. 9 == Home Visit. 10 == Proof of Payment (i.e. deposit slip / atm slip / payslip / etc.)
file_name | string (255) | File name including a random uuid at the front of the file name
file_hash | string (255) | sha256 of the uploaded file
file_size | integer | Size of the uploaded file
created_at | datetime | Datetime in GMT that the file was uploaded

## Download a Document

```shell
curl "https://127.0.0.1.xip.io/api/v1/users/d19bff36-4733-11e5-946b-9ba904d8238e/documents/895b2fcc-545f-11e7-8279-67635886d357"
```

### HTTP Request

`GET https://127.0.0.1.xip.io/api/v1/users/<USER>/documents/<DOCUMENT>`

### URL Parameters

Parameter | Description
--------- | -----------
USER | The UUID of the user who you want to download a document of
DOCUMENT | The UUID of the document you want to download
