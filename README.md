API Documentation

Getting started
You can access the below environments

AUTHORIZATION

Get API access token
Your username & password will be required

POST: https://{domain}:{port}/v1/token

Headers
Authorization = Basic (base64 encoding of key:secret)
Content-Type = application/x-www-form-urlencoded

Parameters
grant_type = client_credentials

Response
token_type (String) : token type
issued_at (Long) : timestamp when token was issued
expires_in (Long) : token expiry time in seconds (3600s/ 1h)
access_token (String) : access token to access other APIs

CALLBACK

Set callback url
This sets the callback url for the business.

POST: https://{domain}:{port}/v1/service/callback

Headers
Authorization = Bearer (token)
Content-Type = application/json

Response
code (Integer) : response status
message (String) : response message
