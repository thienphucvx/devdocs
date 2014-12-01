---
layout: default
group: config-guide
subgroup: Authorization
title: Authorization
menu_title: Authorization
menu_order: 1
menu_node: parent
github_link: config-guide/integration/cg-authorization.md
---

<h2 id="oauth-endpoints">OAuth endpoints</h2>

The authentication endpoints are:

* `/oauth/token/request`. Used to retrieve the *pre-authorized token*, or request token.
* `/oauth/token/access`. Used to retrieve the *access token*.

To use the simple form for authentication, add the `/simple` endpoint to the authentication endpoint. For example: `/oauth/authorize/simple`.

<h2 id="oauth-signature">OAuth signature</h2>

To construct the signature base string, construct a concatenated string of URL-encoded attributes and parameters.

* Use the ampersand (`&`) character to concatenate these attributes and parameters into the string:

    1. HTTP method
    1. URL
    1. oauth_nonce
    1. oauth_signature_method
    1. oauth_timestamp
    1. oauth_version
    1. oatuh_consumer_key
    1. oauth_token

* To generate the signature, you must use the HMAC-SHA1 signature method.

* The signing key is the concatenated values of the consumer secret and token secret separated by the ampersand (`&`) character (ASCII code 38), even if empty. Each value must be encoded through parameter encoding.

<h2 id="http-post">HTTP POST with OAuth credentials</h2>

When the add-on is installed in a Magento instance, Magento creates `oauth_consumer_key` and  `oauth_consumer_secret` and passes these to the add-on through HTTP POST to a HTTP endpoint defined by the add-on.

HTTP POST must contain these attributes:

* Magento store URL. For example, `http://mystore.magentogo.com`
* Magento store API endpoint base: `http://mystore.magentogo.com/api`
* OAuth consumer key
* OAuth consumer key secret

<h2 id="pre-auth-token">Get a pre-authorized token</h2>

The first step to authenticate the user is to retrieve a request token from Magento. This is a temporary token that is exchanged for the Access Token.
Endpoint: 	/oauth/token/request
Method: 	POST
Sample Response: 	oauth_token=4cqw0r7vo0s5goyyqnjb72sqj3vxwr0h&oauth_token_secret=rig3x3j5a9z5j6d4ubjwyf9f1l21itrr

The following request parameters should be present in the `Authorization` header:

*     oauth_consumer_key - the Consumer Key value, retrieved after the registration of the application.
*     oauth_nonce - a random value, uniquely generated by the application.
*     oauth_signature_method - name of the signature method used to sign the request. Can have one of the following values: HMAC-SHA1
*     oauth_signature - a generated value (signature).
*     oauth_timestamp - a positive integer, expressed in the number of seconds since January 1, 1970 00:00:00 GMT.
*     oauth_version - OAuth version.

<h2 id="get-access-token">Get an access token</h2>

Endpoint: 	/oauth/token/access
Method: 	POST
Sample Response: 	oauth_token=0lnuajnuzeei2o8xcddii5us77xnb6v0&oauth_token_secret=1c6d2hycnir5ygf39fycs6zhtaagx8pd

The following components should be present in the `Authorization` header:

*     oauth_consumer_key - the Consumer Key value provided after the registration of the application.
*     oauth_nonce - a random value, uniquely generated by the application.
*     oauth_signature_method - name of the signature method used to sign the request. Can have one of the following values: HMAC-SHA1, RSA-SHA1, and PLAINTEXT.
*     oauth_signature - a generated value (signature).
*     oauth_timestamp - a positive integer, expressed in the number of seconds since January 1, 1970 00:00:00 GMT.
*     oauth_token - the oauth_token value (Request Token) received from the previous steps.
*     oauth_verifier - the verification code that is tied to the Request Token.

The response contains these response parameters:

*     oauth_token - the access tokenthat provides access to protected resources.
*     oauth_token_secret - the secret that is associated with the Access Token.

<h2 id="oauth-error-codes">OAuth error codes</h2>

When the third-party application makes an invalid request to Magento, the following OAuth-related errors can occur:

<table style="width:100%">
   <tr bgcolor="lightgray">
      <th>HTTP code</th>
      <th>Error code</th>
      <th>Text representation</th>
      <th>Description</th>
   </tr>
   <tr>
      <td>400</td><td>1</td><td>version_rejected</td><td>The oauth_version parameter does not correspond to the "1.0" value.</td></tr>
<tr>
      <td>400</td><td>2</td><td>parameter_absent</td><td>A required parameter is missing in the request. The name of the missing parameter is specified additionally in the response.</td></tr>
<tr>
      <td>400</td><td>3</td><td>parameter_rejected</td><td>The type of the parameter or its value do not meet the protocol requirements (for example,  array is passed instead of the string).</td></tr>
<tr>
      <td>400</td><td>4</td><td>timestamp_refused</td><td>The timestamp value in the oauth_timestamp parameter is incorrect.</td></tr>
<tr>
      <td>401</td><td>5</td><td>nonce_used</td><td>The nonce-timestamp combination has already been used.</td></tr>
<tr>
      <td>400</td><td>6</td><td>signature_method_rejected</td><td>The signature method is not supported. The following methods are supported: HMAC-SHA1.</td></tr>
<tr>
      <td>401</td><td>7</td><td>signature_invalid</td><td>The signature is invalid.</td></tr>
<tr>
      <td>401</td><td>8</td><td>consumer_key_rejected</td><td>The Consumer Key has incorrect length or does not exist.</td></tr>
<tr>
      <td>401</td><td>9</td><td>token_used</td><td>An attempt of authorization of an already authorized token or an attempt to exchange a not temporary token for a permanent one.</td></tr>
<tr>
      <td>401</td><td>10</td><td>token_expired</td><td>The temporary token has expired. At the moment, the mechanism of expiration of temporary tokens is not implemented and the current error is not used.</td></tr>
<tr>
      <td>401</td><td>11</td><td>token_revoked</td><td>The token is revoked by the user who authorized it.</td></tr>
<tr>
      <td>401</td><td>12</td><td>token_rejected</td><td>The token is not valid, or does not exist, or is not valid for using in the current type of request.</td></tr>
<tr>
      <td>401</td><td>13</td><td>verifier_invalid</td><td>The confirmation string does not correspond to the token.</td></tr>
</table>

<h2 id="web-api-access">Web API access</h2>

After the add-on is registered and authorized to make API calls, add-ons can invoke Magento web API's using the access token.

<h2 id="web-api-access-sequence">Web API access sequence (Magento CE, EE & Go)</h2>

Endpoint: 	/api/rest/v1/products/1234
Method: 	GET

The following components should be present in the request header:

*     oauth_consumer_key - the Consumer Key value provided after the registration of the application.
*     oauth_nonce - a random value, uniquely generated by the application.
*     oauth_signature_method - name of the signature method used to sign the request. Can have one of the following values: HMAC-SHA1, RSA-SHA1, and PLAINTEXT.
*     oauth_signature - a generated value (signature).
*     oauth_timestamp - a positive integer, expressed in the number of seconds since January 1, 1970 00:00:00 GMT.
*     oauth_token - the oauth_token value (Acess Token) received from the previous steps.