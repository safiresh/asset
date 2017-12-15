## Bluescape OAuth 2.0 APIs Node.js Client
Bluescape OAuth 2.0 supports the following flows.

* [Implicit Grant Flow](http://tools.ietf.org/html/draft-ietf-oauth-v2-31#section-4.2) (for apps with servers that can store persistent information).
* [Client Credentials Flow](http://tools.ietf.org/html/draft-ietf-oauth-v2-31#section-4.4) (the client can request an access token using only its client credentials)

## Requirements
Node Auth client library is tested against the latest minor Node versions: >=4.

### Installation
This library is distributed on `npm`. To add it as a dependency,
run the following command:

``` sh
$ npm install bluescape-auth-library --save
```

### Options
Bluescape OAuth Library accepts an object with the following valid params.

* `client` - required object with the following properties:
  - `id` - Service registered client id.
  - `secret` - Service registered client secret.
  - `redirectUri` - Client redirection endpoint. Required.
 
## APIs
### `clientCredentials`
#### `getToken([options],[callback])`
* `options` - an optional object with the following optional keys:
  - `grantType` - (Optional) Defaults to `client_credentials`.

### `implicit`
#### `getToken([options],[callback])`
* `options` - required object with the following properties.
  - `user` - required object with the following properties.
    * `email` - User email id to authenticate. Required.
    * `password` - User password to authenticate. Required.
  - `response_type` - Optional option to authorise and get the access token (default => `id_token token`).
  - `nonce` - Optional option to authorise and get the access token (default => `nonce`).

### Usage
### Client Credentials Flow

This flow is suitable when authenticating through implicit OAuth.

```javascript
const AuthLib = require('bluescape-auth-lib');

const credentials = {
  client: {
    id: 'XXX',
    secret: 'YYYY',
  },
};

const authLib = new AuthLib(credentials);

// Callbacks
// Get the access token object for the client
authLib.clientCredentials.getToken({}, (error, accessToken) => {
  if (error) {
    return console.log('Client credential - Access Token Error', error.message);
  }
  return console.log(accessToken);
});


// Promises
// Get the access token object for the client
authLib.clientCredentials
.getToken()
.then((accessToken) => {
  console.log(accessToken);
})
.catch((error) => {
  console.log('Client credential - Access Token error', error.message);
});
```

### Implicit OAUTH Flow

```javascript
const AuthLib = require('bluescape-auth-lib');

const credentials = {
  client: {
    id: 'XXXX',
    redirectUri : 'http://ABCD',
  }
};

const authLib = new AuthLib(credentials);
const options = {
  user: {
    email: 'xxx@yyy.com',
    password: 'zzz',
  },
};

// Callbacks
// Get the access token through the implicit oauth
authLib.implicit.getToken(options, (error, accessToken) => {
  if (error) {
    return console.log('Implicit OAUTH Access Token Error', error.message);
  }
  return console.log(accessToken);
});

// Promises
// Get the access token through the implicit oauth
authLib.implicit
.getToken(options)
.then((accessToken) => {
  console.log(accessToken);
})
.catch((error) => {
  console.log('Implicit OAUTH Access Token error', error.message);
});

```

### Errors

Joi validation Error raised when a input params incorrect.
Exceptions are raised when a 4xx or 5xx status code is returned.

    HTTPError

Through the error message attribute you can access the JSON representation
based on HTTP `status` and error `message`.

```javascript
// Callbacks
authLib.clientCredentials.getToken({}, (error, token) => {
  if (error) {
    return console.log(error.message);
  }
});

// Promises
authLib.clientCredentials
  .getToken({})
  .catch((error) => {
    console.log(error.message);
  });

// => {"name":"Error","status":401,"message":"Unauthorized","context":"Unauthorized"}
```
