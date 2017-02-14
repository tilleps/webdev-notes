
## Developer Notes ##

JWT Format:
grant_type "urn:ietf:params:oauth:grant-type:jwt-bearer"


- DO NOT have client_secret accessible to public (request code instead)
- https://localhost:3000/dialog/authorize?redirect_uri=https://localhost:3000&response_type=code&client_id=abc123&scope=offline_access

- http://localhost:3000/dialog/authorize?redirect_uri=https://localhost:8080/receivecode&response_type=code&client_id=abc123&scope=offline_access

- https://authserver/dialog/authorize?redirect_uri=https://apiserver/receivetoken&response_type=code&client_id=abc123&scope=offline_access


      code          : req.query.code,
      redirect_uri  : config.authorization.redirectURL,
      client_id     : config.client.clientID,
      client_secret : config.client.clientSecret,
      grant_type    : 'authorization_code',
      
      

- ACCESS TOKEN - Post to https://localhost:3000/oauth/token
  code=7HMEo1VA1xVS6EkJ&redirect_uri=https://localhost:3000&client_id=abc123&client_secret=ssh-secret&grant_type=authorization_code
  
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
  "audience": "abc123",
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
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3RpbGxlcHMuYXV0aDAuY29tLyIsInN1YiI6ImFkWDZSR3F0Zk9jazc5R2R6ekFadW1tY0t0bXpkTXg5QGNsaWVudHMiLCJhdWQiOiJodHRwczovL3NlcnZpY2Vpbm5vdmF0aW9uLnRlc2xhbW90b3JzLmNvbS9hcGkvIiwiZXhwIjoxNDgzODI2NTUyLCJpYXQiOjE0ODM3NDAxNTIsInNjb3BlIjoiY3JlYXRlOnRvb2xzIHJlYWQ6dG9vbHMifQ.2Z0r9A7zVAHSUXzZ-JBnwPt6INjaJgGwI2Qu92fu2Xk",
  "token_type": "Bearer"
}
```


```
{
  "iss": "https://tilleps.auth0.com/",
  "sub": "adX6RGqtfOck79GdzzAZummcKtmzdMx9@clients",
  "aud": "https://serviceinnovation.teslamotors.com/api/",
  "exp": 1483826989,
  "iat": 1483740589,
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
SAMLRequest=nVPLbsIwEPyVyPeQB4WCRUAUVBWpjwjSHnqpjLMUS46deh2gf18nPMShcOAUeXd2dnZ2MxjtCultwKDQKiFRKySj4QBZIUs6ruxazeGnArSegymkTSIhlVFUMxRIFSsAqeV0MX55pnErpKXRVnMtiTebJuSrDb0263DWiZfdvLdctUPeJd7HsaGrcEDECmYKLVPWhcLo3g8jP77L4oi2
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
                  <DigestValue>9K6jj5vcP8Y/SwBFwKuLQf0NUq9umk1Dv/rGxfVVC1k=</DigestValue>
              </Reference>
          </SignedInfo>
          <SignatureValue>BVuLzRcoqk2Gy1cE2u3VcmdiJ9V/c7URjp6cbjaT4H29fvaSBll58ZqxXfxU80cMi77AdvIhs0RXEJnusPWjQQtT57eKYfbWteLD0WmlVqKJz5uvmUa/H/VJDDMnHv6RiIjLaCDX39oFwSViNKsQGcaYZKWla43JktUQ9pTLGLeHNV7eEtaXA9VMo2O8eKQNAg1exR87m6UQLQIUYgpuSNQRjNhw9pXKRSFcy+ItHy1ahgkqbde2O4Xoa2TErzKJmuZB1yQXd+Q0Tf5OQGZqgXV/w/MF4OPEJxWeMuFxrXqgkdxwFXD2Bmot0di1Ns8oiEHoHD7lwDKxi5eqw7AYjw==</SignatureValue>
          <KeyInfo>
              <X509Data>
                  <X509Certificate>MIIEMDCCAxigAwIBAgIJAMFalZXunCIBMA0GCSqGSIb3DQEBBQUAMG0xCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1TYW4gRnJhbmNpc2NvMRAwDgYDVQQKEwdKYW5reUNvMR8wHQYDVQQDExZUZXN0IElkZW50aXR5IFByb3ZpZGVyMB4XDTE3MDEyNDAwMDY0MloXDTM3MDExOTAwMDY0MlowbTELMAkGA1UEBhMCVVMxEzARBgNVBAgTCkNhbGlmb3JuaWExFjAUBgNVBAcTDVNhbiBGcmFuY2lzY28xEDAOBgNVBAoTB0phbmt5Q28xHzAdBgNVBAMTFlRlc3QgSWRlbnRpdHkgUHJvdmlkZXIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDP288rg5PndoDbpgcO7/B93BAnA06eS7WNLnT5Nx//H1N1bZv8T2zMgSLBVsEe08+1/fRnfe1lUtiTWqKiaFw8UIrLqf26kqRB5OfImyTqXH88FI8tuZW3ZFLailORNYiJAFElzyv1oaCSieGbrcVuy6xVi01jgyCVI7VkBBX5fubyLXN7muLoH0CtADvSKjRT6jtPZgxfK3ynqRerPwxIWxWkIP29beknnG88wA7SFPgC2VUshcVPH7z4963OtOZygDq46P6E2dYGYj1MXvGpZfQaJ4YTWyWfZYL3hCmZHoaA0dFLqbxj7MoZB92nOLxHZ0ty358gM1CO6yU1BoF5AgMBAAGjgdIwgc8wHQYDVR0OBBYEFGS/D5iSDWk+3hE84V/+Ec5MHh9tMIGfBgNVHSMEgZcwgZSAFGS/D5iSDWk+3hE84V/+Ec5MHh9toXGkbzBtMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNU2FuIEZyYW5jaXNjbzEQMA4GA1UEChMHSmFua3lDbzEfMB0GA1UEAxMWVGVzdCBJZGVudGl0eSBQcm92aWRlcoIJAMFalZXunCIBMAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEFBQADggEBAI0xpPnT4n29eQ12mtxa3APQlGId9ER27gCj/VJZ6Yk2jhCn52BqWCviV4dsywh1iWd6dJsJ/7tKraYqodzoV3094Zb3tURc1tY7tV5IH3oaj0z7E00Ly94M/dDOe8RcDVcb/4y+nNVONTWtAFomyUGiQ/rDdkEbGolaHX4rCOGUkGvl6coSA5e8H7XDl0iRcxwlwl6t4jRXV72mSBE8RLEDfIqqXkfFlT+jgUUQtIyHXb8GpBxoKwhfNnwpymPwyJcMQU+CoZWyMqxQCEqKb6Am6zj6/bL29DOwT1lWFq6vCFSy4LZlsv4Oiwb5aUjR+xjy3rY2WXDJxlnIBBP+yac=</X509Certificate>
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
  

