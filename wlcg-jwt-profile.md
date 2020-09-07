# WLCG JWT profile compliance

## Basic JWT verification recommendations

https://github.com/WLCG-AuthZ-WG/common-jwt-profile/blob/master/profile.md#token-verification
https://tools.ietf.org/html/rfc7519#section-7.2

## OAuth resource server 

https://tools.ietf.org/html/rfc6749#section-1.1

"The server hosting the protected resources, capable of accepting
and responding to protected resource requests using access tokens"

Basically: any service that accepts access tokens for authorization.

Example: SEs (StoRM, XRooTD, dCache...), RUCIO, FTS

Requirements:

- Ability to extract an access token from an incoming HTTP request
- Ability to parse and validation the incoming access token
  - identify if the token is issued by a trusted authorization server
  - verify temporal validity
  - verify signature
- Ability to honour access token restrictions
- Ability to map scopes in the token to local authZ
- Ability to map group information in the token to local authZ

## OAuth/OpenID Connect client 

"An application making protected resource requests on behalf of the
resource owner and with its authorization.  The term "client" does
not imply any particular implementation characteristics (e.g.,
whether the application executes on a server, a desktop, or other
devices)."

Example: oidc-agent, StoRM, dCache, FTS, RUCIO

- Ability to store client credentials securely
- Ability to start and manage an OAuth/OpenID Connect flow to obtain tokens
  from the Authorization server
- Ability to parse and validate ID tokens resulting from OpenID Connect
  authentication flows in compliance with the OpenID connect spec
- Ability to honour audience restrictions

## Audience restrictions

Quoting:

https://github.com/WLCG-AuthZ-WG/common-jwt-profile/pull/3/files#diff-3b9a1a89335a911c0c69b58b043febbeR328

Relying parties are recommended to accept audiences based on the endpoint name
most commonly associated with the provided service. That is, if a client would
want to access the resource `https://wlcg.example.com/base/file.root`, then the
recommended audience for the relying party at this endpoint would be:

```
https://wlcg.example.com
```

The audience SHOULD be normalized according to Section 6 of RFC 3986; that is,
trailing slashes are discouraged and default ports should not be included.
However, the comparison against the value MUST be done as a case sensitive
string as specified in Section 4.1.3 RFC 7519.

Note that a given relying party may accept several audiences.  For example, a
storage server SHOULD accept an audience based on a load balanced endpoint it
participates in (e.g., `https://redirector.example.com`) in addition to its
local hostname (`https://server1.example.com`).  The user SHOULD NOT need to
request a new token for each HTTP redirect within the same logical service.

Audiences of this form are preferred (as opposed to a human-readable "site
name" such as `WLCG_Site_Foo`) because an end-user can often derive the correct
audience from a given URL, allowing them to request an appropriate token.  Site
names are often community-specific and would require the user to maintain a
lookup table from endpoint to audience.

If the relying party provides a non-HTTPS-based service, a URI should be used.
For example, a HTCondor-CE running at `condor.example.com` may use an audience
of the form `condor://condor.example.com`.

### Requesting audiences from IAM 

TBD

## Scope-based authorization

https://github.com/WLCG-AuthZ-WG/common-jwt-profile/blob/master/profile.md#capability-based-authorization-scope

### Requesting scopes 

## Group-based authorization

https://github.com/WLCG-AuthZ-WG/common-jwt-profile/blob/master/profile.md#group-based-authorization-wlcggroups

### Requesting groups

TBD
