
## Developer Notes ##

Postman Redirect URI:
https://www.getpostman.com/oauth2/callback


JWT Format:
grant_type "urn:ietf:params:oauth:grant-type:jwt-bearer"


- DO NOT have client_secret accessible to public (request code instead)
- https://localhost:3000/dialog/authorize?redirect_uri=https://localhost:3000&response_type=code&client_id=myClientID&scope=offline_access

- http://localhost:3000/dialog/authorize?redirect_uri=https://localhost:8080/receivecode&response_type=code&client_id=myClientID&scope=offline_access

- https://authserver/dialog/authorize?redirect_uri=https://apiserver/receivetoken&response_type=code&client_id=myClientID&scope=offline_access


      code          : req.query.code,
      redirect_uri  : config.authorization.redirectURL,
      client_id     : config.client.clientID,
      client_secret : config.client.clientSecret,
      grant_type    : 'authorization_code',
      
      

- ACCESS TOKEN - Post to https://localhost:3000/oauth/token
  code=7HMEo1VA1xVS6EkJ&redirect_uri=https://localhost:3000&client_id=myClientID&client_secret=myClientSecret&grant_type=authorization_code
  
  Add if you want refresh token
  scope=offline_access
  
  
  content-type to: application/x-www-form-urlencoded

  {
    "access_token": "nvhxw0MQf9CPbT2fr8FN4uUvGCSmCE2MiTIo14mniaaI5lJiLUwhs1OJc1d6blyJVFfPjlyFX0BhmCgJicpCdfoxJPbsYzl34FLKQDfRjC4uB9F9LlPoMmRrd98g8HN1pqCs6LYMNV24QXfvar87bSKx8f1K5F1gyWsgHbiaa9DpyHNC0NmaXz1ojDprw0aCfGlbZ6osvMng9tTWR1LmegtEJrHslPvRIq0CPXiS2l81VPAPNLUgDYivSnzEY0q7",
    "refresh_token": "lDsloBxS9wdqxgIEhZ8V3b1P3yw5WugnCiJPbHSKTo5PV94d7vU9gw2sEUKZrGqs0pDm5aZ6y6kLt3MtdMGPabn9cYVvyb6eKSnjiBprO9X0d1fKF6jfYEYJl3yFPXgAahOM6F2GdimGFc0sYAkvR4vIivHM1Z1vON2lMEYwe3CIQ3SkO9li6DZ7glbN9yynePvUz6BmfjQA7EJ3XBj1wgXhQFt2zaB1p6H5SvgVoSs2pYz3FeOeJMUHx78XVxit",
    "expires_in": 3600,
    "token_type": "bearer"
  }  
  
  
  Use AccessToken to access APIs
  Authorization: Bearer
  
- REFRESH Token - POST to: https://localhost:3000/oauth/token
  grant_type=refresh_token&refresh_token=
  


API SERVER
  
  - Check for Cache for AccessToken / Query authentication server if none found
  - Read Token info on the server: https://authserver/api/tokeninfo?access_token=
  - Read expiration date, delete expired ones, extend expiration date by expires_in seconds
  - Request AccessToken with refresh token
  - Store new token
  
  - Check session is authorized (req.session.isAuthorized)
  - Redirect to https://authserver/dialog/authorize with redirectURL to https://apiserver/receivetoken
  
  
  

### Flow Developer Notes ###

Authorize Flow
  Existing User?
  Check Gatekeeper -> Import


Bearer asks for AccessToken
API -> if (!AccessToken) {
  Redirect to Authentication Server to Login
  Success
}


Authorize Server -> Generate Access Token

Successful
```
{
  "audience": "myClientID",
  "expires_in": null
}
```

Failed
```
{
  "error": "invalid_token"
}
```




APIs

- API ID
- Name (User Friendly)
- Identifier (Audience)
- Token Expiration (Seconds)
- Signing Algorithm
- Signing Secret


  Authorize Clients
    


Clients

- Name
- Domain
- Client ID
- Client Secret
- Client Type (Native, Non Interactive Client, Regular Web Application, Single Page Application)
- Token Endpoint Authentication Method (Post, Basic, None)
- Allowed Callback URLs
- Allowed Logout URLs
- Allowed Origins (CORS)
- JWT Expiration (seconds)
- Use TeslaAuth instead of the IdP to do Single Sign On


1. Request token
POST /oauth/token
```
{
  client_id: "",
  client_secret: "",
  audience: "",
  grant_type: "client_credentials"
}
```

2. Response
```
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3RpbGxlcHMuYXV0aDAuY29tLyIsInN1YiI6Ikc1a1JuQjJRQXF0VDhUcFZSdTJQYjU1Mmt3a1dDZDhnakVaOHVAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vc29tZXNlcnZpY2UueW91cmRvbWFpbi5jb20vYXBpLyIsImV4cCI6MTQ4MzgyNjU1MiwiaWF0IjoxNDgzNzQwMTUyLCJzY29wZSI6ImNyZWF0ZTp0b29scyByZWFkOnRvb2xzIn0.BTT5yr6BXwWVn1kp6LdtaehNgRfRVK55H5-gipeILjo",
  "token_type": "Bearer"
}
```


```
{
  "iss": "https://tilleps.auth0.com/",
  "sub": "G5kRnB2QAqtT8TpVRu2Pb552kwkWCd8gjEZ8u@clients",
  "aud": "https://someservice.yourdomain.com/api/",
  "exp": 1483826552,
  "iat": 1483740152,
  "scope": "create:tools read:tools"
}
```

Reserved
  exp   expiration Date()
  nbf   Not before time
  iat   Issued at Date()
  iss   Issuer [string]
  aud   API ID
  prn   Principal (Subject)
  jti   JWT ID
  typ   Type of contents
  name
  given_name
  family_name
  middle_name
  nickname
  preferred_username
  profile
  picture
  website
  email
  email_verified
  gender
  birthdate
  zoneinfo
  locale
  phone_number
  phone_number_verified
  address
  updated_at
  azp  Authorized party
  nonce  Value used to associate a client session with an ID token
  auth_time
  sub_jwk Public key used to check the signature of an ID token
  cnf Confirmation
  

Public
  https://www.iana.org/assignments/jwt/jwt.xhtml
  
Private
  

### Scopes Developer Notes ###s

openid {
  iss
  sub
  aud
  exp
  iat
}

openid email  {
  email
  email_verified
}

openid profile {
  name
  family_name
  given_name
  middle_name
  nickname
  preferred_username
  profile
  picture
  website
  gender
  birthdate
  zoneinfo
  locale
  updated_at
}

offline {
  refresh_token
}



### SAML  Developer Notes ###

- https://en.wikipedia.org/wiki/SAML_2.0#Web_Browser_SSO_Profile
- https://auth0.com/docs/protocols/saml/saml-configuration
- https://auth0.com/docs/protocols/saml/samlsso-auth0-to-auth0#4-add-your-service-provider-metadata-to-the-identity-provider
- https://www.npmjs.com/package/saml-toolkit
- https://en.wikipedia.org/wiki/SAML_2.0#SAML_2.0_Assertions
- https://github.com/leandrob/saml20

InResponseTo: Randomly generated ID by SAML Requester


**Redirect from SP to IDP:**
SAMLRequest=bdaaTZzpJHJCcm3dn3TK8nKUmEZHqM46fDpr8UPDB5GvEAcTheR6UM8GSVJ5cY5jQ3rHc3d4UHnBH5URmmxM4E2xuQCY7PNwMub5Vp4XypaULs4UMZuEUJBT592p7YAubdaaTZzpJHJCcm3dn3TK8nKUmEZHqM46fDpr8UPDB5GvEAcTheR6UM8GSVJ5cY5jQ3rHc3d4UHnBH5URmmxM4E2xuQCY7PNwMub5Vp4XypaULs4UMZuEUJBT592p7YAu
```xml
  <?xml version="1.0"?>
  <samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_3e83a5ca52b6d8bf30c6" Version="2.0" IssueInstant="2017-01-24T21:39:52.975Z" ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" AssertionConsumerServiceURL="http://localhost:8080/saml/consume" Destination="http://localhost:8000/saml/v2/assert">
  <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">urn:passport-saml</saml:Issuer>
  <samlp:NameIDPolicy xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress" AllowCreate="true"/>
  <samlp:RequestedAuthnContext xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" Comparison="exact">
    <saml:AuthnContextClassRef xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</saml:AuthnContextClassRef>
  </samlp:RequestedAuthnContext>
  </samlp:AuthnRequest>
```


```xml
  <?xml version="1.0"?>
  <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_41dacce222880ac81fd4" InResponseTo="_beb5fb8c6eca678dc8e7" Version="2.0" IssueInstant="2017-01-25T18:30:55Z" Destination="urn:passport-saml">
      <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">undefined</saml:Issuer>
      <samlp:Status>
          <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
      </samlp:Status>
      <saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" Version="2.0" ID="_4rGVjVytnSjgB6DFpj5BBEkOL9xsdHin" IssueInstant="2017-01-25T18:30:55.912Z">
          <saml:Issuer/>
          <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
          <SignedInfo>
              <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
              <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
              <Reference URI="#_4rGVjVytnSjgB6DFpj5BBEkOL9xsdHin">
                  <Transforms>
                      <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                      <Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                  </Transforms>
                  <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
                  <DigestValue>rGxfVVC1k/bzjDTaR5uJ2RPz5mmgfQ/vcP8Y=</DigestValue>
              </Reference>
          </SignedInfo>
          <SignatureValue>apKrFrNPjqYBm4CVYPYCqeC5AxGazNhfwfNz5nUdjYjrLSQEKNKPd7vWmfNrMpXZmELeMfnLgSdh4fcG6xULTJnxEJUGLB5hj8H7pXTpQNF6FLfWgAEURgWPMNh2TSLgTfEVNKAM8bmnqewXkk4ZXNJZJ52MhXk6W8XQpsSfKbDgvYkP95hfpeZRhKm5r88bPxRUHM6A4gwn9SdxUZ7kG7peyR995tdfBXJgcGnrxbkNYA72CKcKTJ2SS8VQ4GHFapKrFrNPjqYBm4CVYPYCqeC5AxGazNhfwfNz5nUdjYjrLSQEKNKPd7vWmfNrMpXZmELeMfnLgSdh4fcG6xULTJnxEJUGLB5hj8H7pXTpQNF6FLfWgAEURgWPMNh2TSLgTfEVNKAM8bmnqewXkk4ZXNJZJ52MhXk6W8XQpsSfKbDgvYkP95hfpeZRhKm5r88bPxRUHM6A4gwn9SdxUZ7kG7peyR995tdfBXJgcGnrxbkNYA72CKcKTJ2SS8VQ4GHF==</SignatureValue>
          <KeyInfo>
              <X509Data>
                  <X509Certificate>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</X509Certificate>
              </X509Data>
          </KeyInfo>
      </Signature>
      <saml:Subject>
          <saml:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified">1</saml:NameID>
          <saml:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
              <saml:SubjectConfirmationData NotOnOrAfter="2017-01-25T19:30:55.912Z" InResponseTo="_beb5fb8c6eca678dc8e7"/>
          </saml:SubjectConfirmation>
      </saml:Subject>
      <saml:Conditions NotBefore="2017-01-25T18:30:55.912Z" NotOnOrAfter="2017-01-25T19:30:55.912Z">
          <saml:AudienceRestriction>
              <saml:Audience>urn:passport-saml</saml:Audience>
          </saml:AudienceRestriction>
      </saml:Conditions>
      <saml:AttributeStatement xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <saml:Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:uri">
      <saml:AttributeValue xsi:type="xs:string">1</saml:AttributeValue>
  </saml:Attribute>
  <saml:Attribute Name="http://schemas.passportjs.com/username" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:uri">
  <saml:AttributeValue xsi:type="xs:string">bob</saml:AttributeValue>
  </saml:Attribute>
  <saml:Attribute Name="http://schemas.passportjs.com/password" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:uri">
  <saml:AttributeValue xsi:type="xs:string">secret</saml:AttributeValue>
  </saml:Attribute>
  </saml:AttributeStatement>
  <saml:AuthnStatement AuthnInstant="2017-01-25T18:30:55.912Z">
      <saml:AuthnContext>
          <saml:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:unspecified</saml:AuthnContextClassRef>
      </saml:AuthnContext>
  </saml:AuthnStatement>
  </saml:Assertion>
  </samlp:Response>
```


## OAuth Developer Notes ##


/auth/example-oauth2orize
/auth/example-oauth2orize/callback


{
  url          : 'http://authserver/',
  tokenURL     : 'oauth/v2/token',                                  // oauth/token
  authorizeURL : 'http://authserver/oauth/v2/authorize',            // http://localhost:8000/dialog/authorize
  tokeninfoURL : 'http://authserver/oauth/v2/token?access_token=',  // http://localhost:8000/api/tokeninfo?access_token=
  redirectURL  : 'http://authclient/authcode',                      // http://localhost:8080/authcode
}
  

