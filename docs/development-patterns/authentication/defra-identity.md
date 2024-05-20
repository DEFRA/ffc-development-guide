# Defra Identity

Defra Identity is a service that provides external user authentication and authorisation for Defra services. It is based on the OAuth 2.0 and OpenID Connect standards and is backed by [Azure B2C](https://learn.microsoft.com/en-us/azure/active-directory-b2c/overview).

Defra Identity supports authentication through a [Government Gateway](https://www.gov.uk/log-in-register-hmrc-online-services) or Rural Payments account.

FCP services should use Defra Identity with a Rural Payments account.  This is because the majority of users will have one and it is only possible to retrieve customer and land data data if the user is authenticated with a Rural Payments account.

> In the future, [GOV.UK One Login](https://www.gov.uk/using-your-gov-uk-one-login) will be available as an authentication method via Defra Identity.  It is expected this will be an opportunity to migrate users from their Rural Payments account to GOV.UK One Login.

> Defra Identity only supports external user authentication and cannot be used for internal users.

## Credentials

When users sign in with a Rural Payments account, they input their Customer Reference Number (CRN) and password.  

The CRN is a unique identifier for the individual.

## Permissions

Typically, user data and permissions exist in the Defra Customer service.  However, for Rural Payments accounts, this data is stored in Siti Agri.

Once a user has been authenticated, a service must retrieve this data from Siti Agri via an API call.  A complication of this is that this API cannot retrieve all permissions for an individual, instead it can only retrieve permissions for a specific organisation the person is associated with.

For this reason, FCP journey's should be based around an organisation, rather than an individual.  A typical journey would be:

1. User logs in with their Rural Payments account
2. User selects an organisation they wish to act on behalf of
3. Service retrieves permissions for that organisation
4. User can then perform actions on behalf of that organisation

## Login page

The login page is part of Defra Identity as opposed to the consuming service.  Whenever a user needs to authenticate, the service should redirect the user to the Defra Identity login page.  Once authenticated the user will be redirected back to the service.  More details on how to implement this are below.

The login page can be customised by the Defra Identity team for each consuming service.

## Organisation picker

To support selection of an organisation, Defra Identity provides an organisation picker.  Similar to the login page, this is owned by Defra Identity and can be customised for each consuming service, however data is limited to SBI, organisation Id, and organisation name.

More details on how to interact with this component are below.

The organisation picker can also be disabled.  This can be useful if no permissions or existing data is required, however, it is expected that the majority of FCP services will require the picker.

## Authorisation Code Flow

Defra Identity uses the [Authorisation Code Flow](https://oauth.net/2/grant-types/authorization-code/) to authenticate users.  This is a secure method of authentication that involves the following steps:

1. User is redirected to the Defra Identity login page
2. User logs in
3. User selects an organisation
4. User is redirected back to the consuming service with an authorisation code
5. Consuming service exchanges the authorisation code for an access token
6. Consuming service stores JWT token in session

## Single Sign on

Defra Identity supports single sign on (SSO).  This means that once a user has authenticated with one service, they will not need to re-authenticate with another service until their session expires.  When SSO is enabled, consuming services should still redirect to Defra Identity for authentication, however, the user will not need to enter their credentials.

SSO session is managed through a session cookie on the Defra Identity domain.  The session cookie will expire after 30 minutes without interaction with Defra Identity or if the user closes the browser.

FCP services should be setup to support SSO.

## Implementing Defra Identity in an FCP service

### Onboard with Defra Identity

Contact the Defra Identity team to onboard your service.

They will provide you with the following information.

- Client ID
- Client Secret
- Service ID
- Well Known URL
- Policy name

> all FCP services should use the `signupsigninsfi` policy to ensure users are authenticated with a Rural Payments account

During onboarding, you will need to provide Defra Identity with the following information:

- Redirect URL for each environment (`GET` endpoint Defra Identity should redirect the user to after authentication and organisation selection)
- Sign out URL for each environment (`GET` endpoint Defra Identity should redirect the user to after sign out)

> Note: a `localhost` and port should also be provided to support local development.  If a URL is not provided to Defra Identity, authentication will not work in that environment.

### Decide session setup

Defra Identity will return a JWT token to the consuming service after authentication.  This token should be stored in FCP service's session.  Teams are free to determine the most appropriate way to manage their session dependent on their use case.

Some potential options are:
- Store the JWT in a session cookie
- Store the JWT in a session store (e.g. Redis) and use a session cookie to reference the session store

FCP services must provide a mechanism to clear the session when the user logs out.

FCP services must automatically end the session if the user closes the browser.

For simplicity, this guide will assume [`@hapi/yar`](https://www.npmjs.com/package/@hapi/yar) is used to manage the overall session, whilst the JWT token is stored in a separate session cookie.

### Understand endpoints

The Well Known URL provided by Defra Identity will provide details of all endpoints in JSON format.  Services can use this to programmatically interact with Defra Identity.

The most important endpoints are:
- `authorization_endpoint` - URL to redirect the user to for authentication
- `token_endpoint` - URL to exchange the authorisation code for an access token
- `end_session_endpoint` - URL to redirect the user to after sign out
- `jwks_uri` - URL to retrieve the public key to verify the JWT token

### Implement login

When a user needs to authenticate, the service should redirect the user to the `authorization_endpoint`.

This redirection should include teh following query parameters:

- `p` - the policy name provided by Defra Identity
- `client_id` - the client ID provided by Defra Identity
- `service_id` - the service ID provided by Defra Identity
- `state` - value to maintain state between the request and the callback as well as protect against CSRF attacks (see below)
- `nonce` - value to protect against token replay attacks (see below)
- `redirect_uri` - the URL to redirect the user to after authentication
- `scope` - always set to `openid offline_access`
- `response_type` - always set to `code`
- `response_mode` - always set to `query`
- `forceReselection` - optional parameter to force the user to reselect an organisation on the picker page, even if they have already selected one previously
- `relationshipId` - optional parameter to preselect an organisation and skip the picker page.  This must be an Organisation Id from Rural Payments
- `prompt` - optional parameter to force the user to reauthenticate if set to true.

Once the user has authenticated and selected an organisation, they will be redirected back to the service with an authorisation code in the query parameters.  The code can then be exchanged for an access token via the `token_endpoint`.

#### State

The `state` parameter is used to maintain state between the request and the callback.  This is important to protect against CSRF attacks.  The service should generate a random value for each request and store it in the session.  The value should be included in the request to Defra Identity and checked against the value in the session when the user is redirected back to the service.

The `state` parameter is a `base64` encoded string.

An example of how to generate the `state` parameter using the [`uuid`](https://www.npmjs.com/package/uuid) npm package is below:

```javascript
const state = Buffer.from(JSON.stringify({
  id: uuidv4(), // Unique identifier for the request
})).toString('base64')

request.yar.set('state', state)
```

#### Nonce / Initialisation Vector

The `nonce` parameter is used to protect against token replay attacks.  The service should generate a random value for each request and store it in the session.  The value should be included in the request to Defra Identity and checked against the value in the session when the user is redirected back to the service.

As a random value is used, the term `nonce` is not strictly accurate and "Initialisation Vector" is a more accurate description.

Unlike the state which is returned direct from Defra Identity, the nonce is included in the final JWT token.

An example of how to generate the `nonce` parameter using the [`uuid`](https://www.npmjs.com/package/uuid) npm package is below:

```javascript
const initialisationVector = uuidv4()

request.yar.set('initialisationVector', initialisationVector)
```

### Handle callback from Defra Identity

Once the user has authenticated and selected an organisation, they will be redirected back to the service with an authorisation code in the query parameters.

The service should validate the `state` property and exchange the authorisation code for an access token via the `token_endpoint`.

Once the token is retrieved, the `nonce` property should be validated and the token stored in session state.

```javascript
// redirect route from Defra Identity
server.route({
  method: 'GET',
  path: '/sign-in-oidc',
  handler: async (request, h) => {
    const { code, state } = request.query

    // validate state
    const sessionStateId = request.yar.get('state')
    const { id } = JSON.parse(Buffer.from(state, 'base64').toString())
    
    if (id !== sessionStateId) {
      throw new Error('Invalid state, possible CSRF attack')
    }

    // get token
    const { payload } = await Wreck.post(`${token_endpoint}?client_id=${clientId}&client_secret=${clientSecret}&code=${code}&grant_type=authorization_code&redirect_uri=${redirectUri}`, {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      json: true
    })

    const { access_token } = payload

    // validate initialisation vector
    const sessionInitialisationVector = request.yar.get('initialisationVector')
    const { nonce } = JSON.parse(Buffer.from(access_token.split('.')[1], 'base64').toString())

    if (nonce !== sessionInitialisationVector) {
      throw new Error('Invalid Initialisation Vector, possible token replay attack')
    }

    // store token in session cookie and redirect user to root
    return h.redirect('/').state('auth_cookie', access_token, {
      ttl: null // session cookie
      encoding: 'none', // JWT is already encoded
      isSameSite: 'Lax',
      isSecure: true, // for local development, set to false
      isHttpOnly: true,
      clearInvalid: false,
      strictHeader: true,
      path: '/' // ensure cookie is available on all routes
    })
  }
})
```

### Return user to specific location after authentication

The `state` can also be used to store additional information such as where to redirect the user to once they have authenticated.  This strategy avoids users ending up in an unexpected fixed location after authentication after moving between services, or clicking links to specific locations.

If using [Hapi.js](https://hapi.dev/), the user's intended destination can be taken from the `request.url.pathname` property and stored in the `state` parameter.  Once authentication is complete, the user can be redirected back to the intended destination.

#### Example

User tries to access `/my-account` but is not authenticated.  The service catches a `401` error and redirects the user to `/sign-in` with a query parameter taken from their intended destination.

```javascript
// plugin to handle 401 errors
server.ext('onPreResponse', (request, h) => {
  if (request.response.isBoom && request.response.output.statusCode === 401) {
    return h.redirect(`/sign-in?redirect=${request.url.pathname}`)
  }
  return h.continue
})
```

```javascript
// sign-in route
server.route({
  method: 'GET',
  path: '/sign-in',
  handler: async (request, h) => {
    const state = Buffer.from(JSON.stringify({
      id: uuidv4(), // Unique identifier for the request
      redirect: request.query.redirect, // Intended destination
    })).toString('base64')

    request.yar.set('state', state)

    return h.redirect(`https://${defraIdentity}?p=${policy}&client_id=${clientId}&service_id=${serviceId}&state=${state}&redirect_uri=${redirectUri}&scope=openid%20offline_access&response_type=code&response_mode=query`)
  }
})
```

```javascript
// redirect route from Defra Identity
server.route({
  method: 'GET',
  path: '/sign-in-oidc',
  handler: async (request, h) => {
    const { code, state } = request.query
    
    // validation of state and token exchange omitted
    
    const { redirect } = JSON.parse(Buffer.from(state, 'base64').toString())

    return h.redirect(redirect) // setting of cookie omitted
  }
})
```

### Set up authentication strategy

A Hapi.js plugin can be used to setup an appropriate authentication strategy.  This plugin should be registered with the server and can be used to protect routes that require authentication.

For example, if storing the JWT in a session cookie, the plugin could utilise the `@hapi/cookie` or `hapi-auth-jwt2` plugins.

The below example uses the `hapi-auth-jwt2` plugin to validate the JWT token.

```javascript
await server.register(require('hapi-auth-jwt2'))
await server.register({
  plugin: {
    name: 'auth',
    register: async (server, options) => {
      server.auth.strategy('jwt', 'jwt', {
        key: async () => {
          const { payload } = await Wreck.get(jwks_uri, {
            json: true
          }) // get public key from Defra Identity, `jwks_uri` is part of well known URL

          const { keys } = payload
          const pem = jwkToPem(keys[0]) // convert to pem format
          return { key: pem }
        },
        validate: async (decoded, request, h) => {
          // validate the decoded token
          return { isValid: true, credentials: { name: `${decoded.firstName} ${decoded.lastName}` } }
        },
        verifyOptions: {
          algorithms: ['RS256']
        }
      })

      server.auth.default({ strategy: 'jwt', mode: 'try' }) // try will attempt to authenticate but not fail if not authenticated.  This allows all routes to have access to the credentials object
    }
  }
})
```

### Project routes

If a `try` authentication strategy is used then all routes will attempt to authenticate the user but not fail if not authenticated.  This allows the service to determine if the user is authenticated and act accordingly.

If a route requires authentication, then mode can be set to `required`.  A `401` error will be thrown if the user is not authenticated.  Catching a `401` error and redirecting the user to the sign in page is a common pattern.

```javascript
// route that requires authentication
server.route({
  method: 'GET',
  path: '/my-account',
  options: {
    auth: {
      strategy: 'jwt',
      mode: 'required'
    }
  },
  handler: async (request, h) => {
    return h.response('My account')
  }
})
```

If no authentication is required, then authentication can be set to false.  For example, the sign in route should be accessible at all times.

```javascript
// route that does not require authentication
server.route({
  method: 'GET',
  path: '/public',
  options: {
    auth: false
  },
  handler: async (request, h) => {
    return h.response('Public')
  }
})
```

### Role based authentication

Typically as well as ensuring the user is authenticated, services will also want to ensure the user has the correct permissions to access a route.  For services using Government Gateway, the JWT token will contain the user's permissions for the selected organisation.

However, for services using Rural Payments, the JWT token will not contain the user's permissions.  Instead, the service will need to retrieve the permissions from Siti Agri via an API call.

#### Azure API Management

Unlike FCP services that are hosted in Azure, Siti Agri is deployed to Crown Hosting.  Connectivity from Azure to Crown Hosting is only possible through an Azure API Management (APIM) service

An endpoint has been made available through APIM to access this API from FCP services running in Azure Kubernetes Service.

`https://<ENVIRONMENT>/rural-payments-vet-visits/v1/SitiAgriApi/authorisation/organisation/<ORGANISATION_ID>/authorisation`

The endpoint requires the following headers:

- `crn` - the user's Customer Reference Number
- `X-Forwarded-Authorization` - the JWT token
- `Authorization` - a `Bearer` token from APIM
- `Ocp-Apim-Subscription-Key` - the subscription key for the APIM service

Before the endpoint can be called, the service must first authenticate with APIM.  This is done by calling the following endpoint:

`https://login.microsoftonline.com/<TENANT>/oauth2/v2.0/token`

This endpoint requires a `form-data` type POST request with query parameters.

The following parameters are common to all FCP services and can be found in Azure Key Vault

- clientId
- clientSecret
- scope

Example using [`@hapi/wreck`](https://www.npmjs.com/package/@hapi/wreck):

```javascript
const Wreck = require('@hapi/wreck')
const FormData = require('form-data')

  const data = new FormData()
  data.append('client_id', `${clientId}`)
  data.append('client_secret', `${clientSecret}`)
  data.append('scope', `${scope}`)
  data.append('grant_type', 'client_credentials')

  return Wreck.post(apimConfig.authorizationUrl, {
    headers: data.getHeaders(),
    payload: data,
    json: true
  })
```

#### Parsing Siti Agri response

Siti Agri will return a response containing all roles and permissions for all users associated with the business.  The service must parse this response to determine if the user has the correct permissions.

Example response:

```json
{
  "data": {
    "personRoles": [{
      "personId": 1000001,
      "role": "Farmer"
    }, {
      "personId": 1000002,
      "role": "Agent"
    }],
    "personPrivileges": [{
      "personId": 1000001,
      "privilege": "View"
    }, {
      "personId": 1000001,
      "privilege": "Edit"
    }, {
      "personId": 1000002,
      "privilege": "Edit"
    }]
  }
}
```

A complexity of this is that all permissions are returned, not just for the logged in user.  The service must filter the response based on `personId`.  `personId` is not included in the JWT token and must be retrieved from a separate APIM endpoint.

`https://<ENVIRONMENT>/rural-payments-vet-visits/v1/person/3337243/summary`

> Note: `3337243` is a fixed value and does not need to be changed.

The endpoint requires the following headers:

- `crn` - the user's Customer Reference Number
- `X-Forwarded-Authorization` - the JWT token
- `Authorization` - a `Bearer` token from APIM
- `Ocp-Apim-Subscription-Key` - the subscription key for the APIM service

Example flow:

1. User logs in with their Rural Payments account
2. User selects an organisation
3. Service retrieves organisation Id from `currentRelationshipId` property in JWT token
4. Service retrieves `personId` via APIM
5. Service retrieves permissions from Siti Agri via APIM
6. Service filters permissions based on `personId`
7. Service persists permissions in session

#### Updating credentials

The roles should be stored in the `scope` property of `credentials` during authentication.  As the JWT does not include the roles, they must be added to the `credentials` object during each request.

```javascript
validate: async (decoded, request, h) => {
  // get roles already retrieved from Siti Agri from session
  const roles = request.yar.get('roles') // must be array
  return { isValid: true, credentials: { name: `${decoded.firstName} ${decoded.lastName}`, scope: roles } }
},
```

#### Protecting routes

Once a role has been mapped to a service specific role, the service can protect routes by using the `scope` property.

If the user does not have the correct role, a `403` error will be thrown and the user will not be able to access the route.  Services can catch this error and redirect the user to an appropriate page.

```javascript
// route that requires authentication and a specific role
server.route({
  method: 'GET',
  path: '/my-account',
  options: {
    auth: {
      strategy: 'jwt',
      scope: ['View'] // role required to access route
    }
  },
  handler: async (request, h) => {
    return h.response('My account')
  }
})
```

### Refresh tokens

Defra Identity will also return a Refresh Token when the user authenticates.  This token can be used to obtain a new Access Token when the current Access Token expires.

The Refresh Token should also be stored in session storage, however due to it's size, it's recommended to store it in a session store (e.g. Redis).

Libraries such as `hapi-auth-jwt2` and `@hapi/cookie` do not support Refresh Tokens out of the box.  The service will need to implement this functionality manually.  By default, both of these libraries will fail validation if the token is expired.  This behaviour can be disabled if the service wishes to refresh the token before or after it expires.

### Sign out

When a user signs out, the service should redirect the user to the `end_session_endpoint`.  This will clear the session cookie on the Defra Identity domain and the user will be signed out.

Similar to the login page, the user will be redirected back to the service after sign out.  The service should clear the session cookie and any other session data at this point.

### Cross service user journeys

As FCP grows in scale, it is expected that users will need to move between services within the same session.  We do not want users to have to re-authenticate or re-select an organisation when moving between services.

Services should therefore be designed so that an `organisationId` can be passed to any route as a query string.  The service will then pass this to Defra Identity as the `relationshipId` parameter during the first login process which will mean the picker page is skipped.

If the user has an active session in Defra Identity, the user will automatically not be asked for credentials.
