# XPath #


```xml
  <?xml version="1.0"?>
  <samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_42b0f2a86ab7c8f6523d" Version="2.0" IssueInstant="2017-01-25T02:45:34.430Z" ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" AssertionConsumerServiceURL="http://localhost:8080/saml/consume" Destination="http://localhost:8000/saml/v2/login/untrustedClient">
  <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">urn:passport-saml</saml:Issuer>
  <samlp:NameIDPolicy xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress" AllowCreate="true"/>
  <samlp:RequestedAuthnContext xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" Comparison="exact">
      <saml:AuthnContextClassRef xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</saml:AuthnContextClassRef>
  </samlp:RequestedAuthnContext>
  </samlp:AuthnRequest>
```


```javascript
  var xml = '[insert xml here]';

  var doc = new dom().parseFromString(xml);

  var nodes = [];

  //  Every node
  nodes = xpath.select('.//*', doc);
  
  //  Issuer (Value)
  nodes = xpath.select("//*[local-name(.)='Issuer' and namespace-uri(.)='urn:oasis:names:tc:SAML:2.0:assertion']/text()", doc);
  
  //  Version (Attribute)
  var nodes = xpath.select("//*[local-name(.)='AuthnRequest' and namespace-uri(.)='urn:oasis:names:tc:SAML:2.0:protocol']/@Version", doc);

  
  if (nodes && nodes.length > 0) {
  
    nodes.map(function (node) {
      console.log('CONTENT', node.textContent);
    });
  
  }

```