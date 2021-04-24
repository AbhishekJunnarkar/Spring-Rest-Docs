# Introduction to API Security
## Examples of common Web Attacks
- Cross site scripting
- Denial of service
- Man in the middle
- Cross-site Request Forgery
- Injection
- Overflow
## Standard Security functions to mitigate API threats
- Rate Limiting
- Message Validation
- Encryption and Signing
- Access Control
## TLS Trust Attacks
- Certificate Authority vulnerablities
- Human vulnerabilities
- Man in the middle

# Introduction to OAuth 2.0
- OAuth 2.0 is built for authorization, not authentication
- OAuth 2.0 excels at delegated authorization like login with google, facebook or twitter
## Purpose of OAuth 2.0 as a framework for authorization
- The OAuth 2.0 is an industry standard authorization protocol that permits a user to grant an applicaiton access t a protected resource without exposing th euser's password credentials
- An OAuth access token is issued and accepted for user authrization at the API endpoint
- e.g. https://abcbank/com/loin/oauth/authorize?client_id=4234234fsfsfrfdaada3qq423423423dadad&scope=transact_hist acct_bal acct_transf
- In the above URL scope is also added to give the user access to a specific level of API's
- OAuth is a delegated access framework for web resources, it is nt a authentication framework.
## Authorization code flow
The OAuth 2.0 authorization code flow involves the following roles and components:
- Resource Owner (End User)
- Client Application
- Authorization Server
- Resource Server and APIs
## Different Authorization grant types
- Authorization code: Use case - Web applications
- Implicit: Use case - Javascript based web applications (single page applications), recommended only for internal applications, not for enterprise applications
- Resource Owner password credentials: Use case - Used where there is high degree of trust between resource owner and client application
- Client Credentials: Use case - Non interactive apps like CLI, Daemon,...Must be used only with 'confidential clients'
## Current Challenges of OAuth 2.0 implementation
- Complexity
- 4 different grant types
- combination of RFC specificaitons
- Complexity price is paid by API providers, NOT API consumers
### Downsides of Standalone OAuth 2.0
- Cannot authenticate the bearer of the access token(If its genuine user of hacker)
- Access token doesnot contain user information
- Trust is assumed for anyone who presents the token (Makes it vulnerable for the man in the middle attack)
# OpenID connect Overview
- In addition to the access token thats issued by OAuth 2.0, an additional ID token(and a refresh token) is issued and used in the authentication flow order for the client application to retrieve resources on behalf of the resource owner.
- OpenID connect applies OAuth to an identity resource
- Identity is a set of attributes (key-values)
- One person can have multiple identities
- OpenID provides authorized access to Identity
  - The ID token acts as a encrypted fingerprint
  - The ID token can be decoded to reveal user information for identity verification
 
 ## ID Token vs Access Token in OpenID connect
 ### ID Token
 - New OAuth token defined in the OpenID Connect
 - Json Web token (JWT) containing claims about authentication status of the end user
 - Indicates status of the authentication
 ### Access token
 - Original OAuth token
 - Provides access to identity resource
 - Can be used by client to retrieve additional user information

## The Open ID connect protocol suite
- The openID connect Protocol suite is divided into the following levels of implementation
  - Minimal(Contains all Core components)
  - Dynamic (Core + Discovery specifications)
  - Complete (Core + dynamic + session management specifications)
- OpenID connect is not same as OpenID 2.0, which is soon going to be deprecated

## OpenID Connect as an authentication layer and how it complements OAuth 2.0
## How IDentity is treated as a resource
## OpenID connects authentication flow
## General specificaitons of the current OpenID Connect protocol suite

# JSON Web Tokens
A token format for securly transmitting information between parties as a ecryption object
- JWT is stateless
- JWT can be used as a ID Token
- JWT cna be used as a Refresh Token
- JWT is signed and encrypted
- JWT is self-contained
- Its compact
- Can be easily passed around
- It has protocal versatility
- It uses common data format
- Langiage-agnostic
- Ideal for REST/HTTP APIs
## JOSE (Javascript object signing and encryption)
JWT is part of Javascript Object signing and Encryption, whihch comprises:
- JWT(JSON Web token)
- JWS(JSON Web signature)
- JWE(JSON Web Encryption)
- JWA(JSON Web Algorithms)
- JWK( JSON web key)

## Anatomy of JWT:
- Header, which specifies authorizationa nd signature
- Payload, which specifies standatd and custom claims
- Signature, whcihs elaborates of th emechanism used to sign the JWT

Some JWT characteristics, which help promote its usage:
- supports signature and encryption
- Stateless and self contained
- Compact and easy to pass around
- Programmic language agnostic, common data format
- Idea for REST/HTTP APIs

Some JWT challenges, which are related to the nature of stateless tokens:
- Token revocation and management
- Token security
- Overhead and performance
- No state, no way to revoke
- Sign off/Log out is not a guranteed action
- Security must be maintained through other means

# Addressing OAuth 2.0 Threats
## Common OAuth 2.0 threat models
OAuth 2.0 threats can occur at any stage of authorization and can target any component in the networking including:
- client and client secrets
- Authorization and resource endpoints
## Client threat models
- Attacker obtaining client secrets
- Attacker obtaining access and refresh tokens
- Attacker phishing for credentials using compromised or embedded browser
- Open redirection on client
- Recommendation(s):- Application access control

## Endpoint threat models
Endpoint threat models take the form of:
- Phishing by counterfeit authorization server
- Interception of traffic to resource server
- User unintentionally grants too much access scope
- Malicious client obtains existing authorization by fraud
- Open redirection
- Recommendation:- Certificate Pinning(RFC-7469), whitelist Redirect URIs, Proof Key for Code Exchange(RFC 7636)

## Token Threat Models
Token threat models can take the form of:
- Evasdropping access tokens
- Obtaining access tokens from authorization server database
- disclosure of client credenatials during transmission
- Obtiaining client secret from authorization server database
- Obtaining client secret by online guessing
- Recommendations:- Token Binding
## Methods and ways to overcome these threats
- Client threat models = Application access control
- Endpoint threat models = Certificate pinning, whitelist redirect urls, Proof key for code exchange
- Token threat models = Token Binding

# API Security Architect
<Add image>
# Annexure:
## Bearer Token
- The Bearer Token is created for you by the Authentication server. When a user authenticates your application (client) the authentication server then goes and generates for you a Token. Bearer Tokens are the predominant type of access token used with OAuth 2.0

### According to RFC6750
- The OAuth 2.0 Authorization Framework: Bearer Token Usage, the bearer token is:
- A security token with the property that any party in possession of the token (a "bearer") can use the token in any way that any other party in possession of it can.

## Bearer Token and Refresh Token
When user requests to the server for a token sending user and password through SSL, the server returns two things: an Access token and a Refresh token.

An Access token is a Bearer token that you will have to add in all request headers to be authenticated as a concrete user.

Authorization: Bearer <access_token>

An Access token is an encrypted string with all User properties, Claims and Roles that you wish. (You can check that the size of a token increases if you add more roles or claims). Once the Resource Server receives an access token, it will be able to decrypt it and read these user properties. This way, the user will be validated and granted along with all the application.

Access tokens have a short expiration (ie. 30 minutes). If access tokens had a long expiration it would be a problem, because theoretically there is no possibility to revoke it. So imagine a user with a role="Admin" that changes to "User". If a user keeps the old token with role="Admin" he will be able to access till the token expiration with Admin rights. That's why access tokens have a short expiration.

But, one issue comes in mind. If an access token has short expiration, we have to send every short period the user and password. Is this secure? No, it isn't. We should avoid it. That's when Refresh tokens appear to solve this problem.

Refresh tokens are stored in DB and will have long expiration (example: 1 month).

A user can get a new Access token (when it expires, every 30 minutes for example) using a refresh token, that the user had received in the first request for a token. When an access token expires, the client must send a refresh token. If this refresh token exists in DB, the server will return to the client a new access token and another refresh token (and will replace the old refresh token by the new one).

In case a user Access token has been compromised, the refresh token of that user must be deleted from DB. This way the token will be valid only till the access token expires because when the hacker tries to get a new access token sending the refresh token, this action will be denied.

Ref: https://stackoverflow.com/questions/25838183/what-is-the-oauth-2-0-bearer-token-exactly/25843058

References:
http://openid.net/connect
