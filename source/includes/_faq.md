# Frequently Asked Questions

## How do I validate a South African Identification Number?

There is no need to understand the composition of the number, just validation which can be done using
sample code from the following URL's:

[Sample code for Java and PHP](http://knowles.co.za/generating-south-african-id-numbers/)

For testing purposes, you can utilise the following ID numbers when adding a user via the API.

| ID Number | Notes |
|---------------|--------------|
| 8510290194083 | Valid ID Number   |
| 9007263637083 | Invalid ID Number - should return an error when using this as an ID number on user creation. |

It is advisable that you also take the first six digits and figure out if the date of birth entered by the customer
matches the date of birth on in their South African ID Document number.


```java
public class IDValidator
{
 private static int generateLuhnDigit(String inputString)
 {
  int total = 0;
  int count = 0;
  for (int i = 0; i < inputString.length(); i++)
  {
   int multiple = count % 2 + 1;
   count++;
   int temp = multiple * Integer.parseInt(""+inputString.charAt(i));
   temp = (int)(Math.floor(temp / 10) + (temp % 10));
   total += temp;
  }

  total = (total * 9) % 10;

  return total;
 };

 public static boolean checkIDNumber(String idNumber)
 {
  String number = idNumber.substring(0, idNumber.length() - 1);
  return generateLuhnDigit(number) == Integer.parseInt(""+idNumber.charAt(idNumber.length() - 1));
 }
}
```

## Using PHP how do I get the JSON back on 400+ status returns with Guzzle?

If you are integrating using PHP, you may want to use [Guzzle](http://docs.guzzlephp.org/en/latest/) for implementing remote web requests.  You can easily install it using composer.

```
composer require guzzlehttp/guzzle:~6.0
```

```php
<?php
        ...

        try {
            $res = $client->request(
                'POST',
                '/api/v1/users',
                [
                    'body' => '{...}',
                    'headers' => [
                        'Authorization' => 'Token token=YOURSYMELATIONTOKEN',
                    ],
                ]
            );
        } catch (\GuzzleHttp\Exception\ClientException $e) {
            if ('400', $e->getResponse()->getStatusCode()) {
                $body = $e->getResponse()->getBody()->getContents();
                ...
			}
			
			...
        }
```

## Using Java how do I get the JSON back on 400+ status returns?

[Workaround from bugs.java.com](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4513568)

> Example Java Code

```java
HttpURLConnection httpConn = (HttpURLConnection)_urlConnection;
InputStream _is;
if (httpConn.getResponseCode() == 200) {
    _is = httpConn.getInputStream();
} else {
     / error from server /
    _is = httpConn.getErrorStream();
}
```
