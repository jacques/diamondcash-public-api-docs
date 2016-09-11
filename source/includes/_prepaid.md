# Prepaid

## List mobile networks

Retreives a list of mobile network providers and fixed line providers who vend
Prepaid Airtime Vouchers.

```shell
curl "https://127.0.0.1.xip.io/api/v1/prepaid/networks"
  -H "Authorization: Token token=YOURTOKEN"
  -H "Content-Type: application/json"
```



```json
{
  "status": "ok",
  "data": [
    {
       "network_id": "1",
       "name": "Vodacom"
    },
    {
       "network_id": "2",
       "name": "MTN"
    },
    {
       "network_id": "3",
       "name": "CellC"
    },
    {
       "network_id": "4",
       "name": "Virgin Mobile"
    },
    {
       "network_id": "5",
       "name": "Neotel"
    },
    {
       "network_id": "6",
       "name": "Telkom Mobile (8ta)"
    }
  ]
}
```

This endpoint retrieves the list of mobile networks.

### HTTP Request


## List available vouchers for a network



## Vend an airtime voucher


```json
```

<aside class="notice">Symelation provides the PIN and voucher serial number from the mobile networks in order to phone their customer support if their are issues with your prepaid vouchers.</aside>

> Example of the SMS message sent out:

```text
Thankyou for buying your airtime from ##BRAND##.  Dial ##USSD## to redeem. Customer Care : ##CUSTOMERCARE## Serial: ##SERIAL##
```


## Vend pinless airtime


<aside class="warning">If you vend pinless airtime to an MSISDN not on the correct network (i.e. trying to vend MTN to a Vodacom MSISDN), it takes up to 24 hours for the status to reflect as failed from the network and the amount for the transaction to be refunded to the user.</aside>

## Lookup Electricity Customer Information

> Example request for retrieving the customer information for the specified meter number:

```shell
```

```json
```

## Vend Electricity STS Token


```shell
```


```json
```
This endpoint retrieves all users.

### HTTP Request

`POST https://127.0.0.1.xip.io/api/v1/users/<USER>/prepaid/electricity`

### Parameters

Parameter | Type | Description
--------- | ---- | -----------
meter_no | integer | STS Meter number
amount | integer | Amount in rands of electricity being purchased
