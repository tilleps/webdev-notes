## Developer Notes ##

JSend JSON Response format
https://labs.omniti.com/labs/jsend


### Authentication ###


Redirect Login
GET http://authserver/oauth/v2/authorize?redirect_uri=http://client/authcode&response_type=code&client_id={myClientID}&scope=offline_access


Access Token (User)

POST http://authserver/oauth/v2/token
Authorization: Basic {myClientID:myClientSecret}
```javascript
{
  "grant_type": "password",
  "username": "bob",
  "password": "secret",
  "scope": "offline_access"
}
```

```javascript
{
  "access_token": "{accessToken}",
  "refresh_token": "{refreshToken}",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```


Access Token (Client)
POST http://authserver/oauth/v2/token
Authorization: Basic {myClientID:myClientSecret}

```javascript
{
  "grant_type": "client_credentials",
}
```
```javascript
{
  "access_token": "{accessToken}",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

Access Token (API Client)
POST http://authserver/oauth/v2/token
Authorization: Basic {myClientID:myClientSecret}
```javascript
{
  "grant_type": "client_credentials",
  "audience": "apiIdentifier"
}
```
```javascript
{
  "access_token": "{jwt}",
  "token_type": "Bearer"
}
```

```javascript
{
  "error":"access_denied",
  "error_description": "Client is not authorized to access \"https://someservice.yourdomain.com/api/2\". You might probably want to create a \"client-grant\" associated to this API. See: https://auth0.com/docs/api/v2#!/Client_Grants/post_client_grants"
}
```

User Information
GET http://authserver/oauth/v2/client
Authorization: Bearer {AccessToken}


Refresh Token
POST http://authserver/oauth/v2/token
Authorization: Basic {myClientID:myClientSecret}
```javascript
{
  "grant_type": "refresh_token",
  "refresh_token": "{refreshToken}"
}
```
```javascript
{
  "access_token": "{accessToken}",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```


### SAML ###

Passport's metadata generation does not appear to have the proper location

```xml
  <EntityDescriptor xmlns="urn:oasis:names:tc:SAML:2.0:metadata" xmlns:ds="http://www.w3.org/2000/09/xmldsig#" entityID="urn:passport-saml" ID="urn_passport_saml">
  <SPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
  <NameIDFormat>
  urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress
  </NameIDFormat>
  <AssertionConsumerService index="1" isDefault="true" Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="http://localhost/saml/consume"/>
  </SPSSODescriptor>
  </EntityDescriptor>
```