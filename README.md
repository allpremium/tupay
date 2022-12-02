# Introduction

The Tupay API is organized around REST. Our API has predictable resource-oriented URLs, accepts form-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

You can use the API in test mode, which doesn't affect your live data or interact with merchants.

The API differs for every account as we release new versions and tailor functionality. Log in to see docs customized to your version of the API, with your test key and data.

Use this page to start your journey towards build amazing applications. You can view the various categories of functionalities on the area to the left, descriptions in the middle and code examples in the area to the right.

## Getting started

You can get started immediately with the APIs here to test them out.
If you would like to go into a full-blown test environment, head on over [here](https://sandbox.tupay.style) to create your account.

## Go Live Checklist

 1. You are configured on production
 2. You have thoroughly tested the integration
 3. Fund your account

# Authorization
<b style="color: #f4b800">POST</b><b>: /v1/b2b/token</b>

The API uses API keys to authenticate requests. You can view and manage your API keys in the Tupay Dashboard.

Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

Authentication to the API is performed via HTTP Basic Auth. Your Account UUID is your username & API Key is your password. Provide a base64 encoded username:password as the basic auth value.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.
    
    curl -X POST
     https://{domain}:{port}/v1/b2b/token
     -H 'Authorization: Basic {base64(username:password)}'
Response Schema

* access_token (String): The token that will be required to access other services
* issued_at (Long): The time issued in milliseconds
* expires_in (Integer): The expiry in seconds

# Idempotent Requests

The API supports [idempotency](https://en.wikipedia.org/wiki/Idempotence) for safely retrying requests without accidentally performing the same operation twice. When creating or updating an object, use an idempotency key. Then, if a connection error occurs, you can safely repeat the request without risk of creating a second object or performing the update twice.

To perform an idempotent request, provide an additional Idempotency-Key: <key> header to the request.

    curl -X POST
     https://{domain}:{port}/v1/b2b/{endpoint}
     -H 'Idempotency-Key: {key}'
Tupay's idempotency works by saving the resulting status code and body of the first request made for any given idempotency key, regardless of whether it succeeded or failed. Subsequent requests with the same key return the same result, including 500 errors.

An idempotency key is a unique value generated by the client which the server uses to recognize subsequent retries of the same request. How you create unique keys is up to you, but we suggest using V4 UUIDs, or another random string with enough entropy to avoid collisions. Idempotency keys can be up to 255 characters long.

Keys are eligible to be removed from the system automatically after they're at least 24 hours old, and a new request is generated if a key is reused after the original has been pruned. The idempotency layer compares incoming parameters to those of the original request and errors unless they're the same to prevent accidental misuse.

Results are only saved if an API endpoint started executing. If incoming parameters failed validation, or the request conflicted with another that was executing concurrently, no idempotent result is saved because no API endpoint began execution. It is safe to retry these requests. 

All POST requests accept idempotency keys. Sending idempotency keys in GET and DELETE requests has no effect and should be avoided, as these requests are idempotent by definition.

# Balance
<b style="color: #f4b800">GET</b><b>: /v1/b2b/balance</b>

This is an object representing your balance. You can retrieve it to see the balance currently on your account.

 ## Balance Request
    curl -X GET
     https://{domain}:{port}/v1/b2b/balance
     -H 'Authorization: Bearer {token}'
Response Schema

* status (Integer): The status of the request
* amount (Double): The balance
* currency (String): The currency code

# Order
<b style="color: #f4b800">POST</b><b>: /v1/b2b/order/{service}</b>

This is an object for you to place an order for services offered by Tupay.
The request for an order will first check if there is sufficient balance in your account and if successful, an order will be placed.
The response will show that the request is in process. A callback url is required so as to receive the final status of the order.
You could also check the status of the transaction using the status object.

    Services:
    * airtime (Safaricom, Airtel, Telkom, Equitel, JTL)
    * water (Nairobi Water)
    * dstv
    * gotv
    * zuku

    curl -X POST
     https://{domain}:{port}/v1/b2b/order/{service}
     -H 'Content-Type: application/json'
     -H 'Authorization: Bearer {token}'
     -H 'Idempotency-Key: {key}'
     -d '"{"account": "{account}", "amount": {amount}}"'
 Parameters

 * account (String): The account e.g phone number
 * amount (Double): The amount required
 * currency (String): The currency code
 * reference (String) Your unique id

Response Schema

* id (String): The transaction id
* status (Integer): The status of the request
* message (String): The response message

# Status
<b style="color: #f4b800">GET</b><b>: /v1/b2b/status/{id}</b>

This is an object for checking the status of a transaction. You will need to save the transaction id from the order response so as to check it's status with this object if needed.

    curl -X GET
     https://{domain}:{port}/v1/b2b/status/{id}
     -H 'Authorization: Bearer {token}'
Parameters

  * id (String): The transaction id or reference

Response Schema

* status (Integer): The status of the request
* message (String): The response message

# Callback
This service enables you to get callbacks if you have set your callback Url.

Response Schema

* id (String): The transaction id
* status (integer): The status of the request
* message (String): The response message
* reference (String): Your unique id
* currency (String): The currency code
* balance (Double): The account balance
* comm (Double): The commission

# Errors
Tupay uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the 2xx range indicate success. Codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted, a charge failed, etc.). Codes in the 5xx range indicate an error with Tupay's servers (these are rare). 

    200 - OK
    400 - Bad Request
    401 - Unauthorized
    402 - Request Failed
    403 - Forbidden
    404 - Not Found
    409 - Conflict
    429 - Too Many Requests
    500, 502, 503, 504 - Server Errors
