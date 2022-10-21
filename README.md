
# Introduction

The Tupay API is organized around REST. Our API has predictable resource-oriented URLs, accepts form-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

You can use the API in test mode, which doesn't affect your live data or interact with merchants. The API key you use to authenticate the request determines whether the request is live mode or test mode.

The API differs for every account as we release new versions and tailor functionality. Log in to see docs customized to your version of the API, with your test key and data.

Use this page to start your journey towards build amazing applications. You can view the various categories of functionalities on the area to the left, descriptions in the middle and code examples in the area to the right. You can switch between your preferred programming language using the tabs that quickly gives you an indication of what you should be incorporating into your code.

## Getting started

You can get started immediately with the APIs here to test them out.
If you would like to go into a full -blown test environment, head on over [here](https://business.tupay.style) to create your account. For a detailed walk-through visit our Guides & tutorials section You will receive your API Keys for test and production from there.

 1. Get a business account with Tupay
 3. Set up a Callback Url
 2. Create an API Key
 4. Consume the API

# Authorization
<b style="color: #f4b800">POST</b>: https://{domain}:{port}/v1/token

The API uses API keys to authenticate requests. You can view and manage your API keys in the Tupay Dashboard.

Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

Authentication to the API is performed via HTTP Basic Auth. Provide your API key as the basic auth username value. You do not need to provide a password.

If you need to authenticate via bearer auth (e.g., for a cross-origin request), use -H "Authorization: Bearer 4eC39HqLyjWDarjtT1zdp7dc" instead of -u 4eC39HqLyjWDarjtT1zdp7dc.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

    curl --request POST
     --url https://{domain}:{port}/v1/token 
     --header 'Authorization: Basic {base64(key:secret)}'

200 Success Response Schema

* access_token: The token that will be required to access other services
* expiry: Expiry in milliseconds

# Airtime
<b style="color: #f4b800">POST</b>: https://{domain}:{port}/v1/b2b/airtime

This service enables you to distribute airtime.

    curl --request POST
     --url https://{domain}:{port}/v1/b2b/airtime 
     --header 'Authorization: Bearer {Token}'
     
200 Success Response Schema

* status: The status of the request
* message: The response message

# Balance
<b style="color: #f4b800">POST</b>: https://{domain}:{port}/v1/b2b/balance

This service enables you to check your balance.

    curl --request POST
     --url https://{domain}:{port}/v1/b2b/balance 
     --header 'Authorization: Bearer {Token}'
     
200 Success Response Schema

* status: The status of the request
* balance: The balance

# Callback
This service enables you to get callbacks if you have set your callback Url.

200 Success Response Schema

* status: The status of the request
* message: The response message



© 2022, Tupay.
