# Authorization flows

## Third-party copy group-based authorization flow

Here RUCIO manages a token-based third-party transfer, managed by FTS, acting on behalf of a user.

### Services involved

- IAM: wlcg.cloud.cnaf.infn.it
- RUCIO: rucio.example
- FTS: fts.example
- SE1: se1.example
- SE2: se2.example

### Example flow

1. User requests token from IAM requesting group claim inclusion, and targets the token to the RUCIO audience

```
> export BEARER_TOKEN=$(oidc-token wlcg -s openid -s wlcg.groups --aud https://rucio.example)
> echo $BEARER_TOKEN | jwt
...
✻ Header
{
  "kid": "rsa1",
  "alg": "RS256"
}

✻ Payload
{
  "wlcg.ver": "1.0",
  "sub": "a1b98335-9649-4fb0-961d-5a49ce108d49",
  "aud": "https://rucio.example",
  "nbf": 1600246822,
  "scope": "openid wlcg.groups",
  "iss": "https://wlcg.cloud.cnaf.infn.it/",
  "exp": 1600250422,
  "iat": 1600246822,
  "jti": "226ba905-fed3-4d12-9ad5-8f328e2c2d36",
  "client_id": "a0deae4e-3843-4167-b967-5dc0eff7b953",
  "wlcg.groups": [
    "/wlcg",
    "/wlcg/xfers"
  ]
}
   Issued At: 1600246822 9/16/2020, 11:00:22 AM
   Not Before: 1600246822 9/16/2020, 11:00:22 AM
   Expiration Time: 1600250422 9/16/2020, 12:00:22 PM
```

2. User submits transfer to RUCIO (whatever this means) authenticating with the
   obtained token

3. RUCIO exchanges this token with one suitable to submit transfers to FTS.
   This is done using the `token exchange` flow. Audience is restricted using
   the `audience` parameter. Below is an example token exchange request:

```
POST /token HTTP/2
Host: wlcg.cloud.cnaf.infn.it
Authorization: Basic u89…
Accept: */*
Content-Length: ...
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:token-exchange
&subject_token=eyJra…HvBfTpM
&audience=https://fts.example
```

4. FTS exchange the received token with a couple of tokens suitable to manage
   the transfer. This is again done using the `token exchange` flow. Audience
   is restricted using the `audience` parameter. Below is an example of token
   exchange request:

```
POST /token HTTP/2
Host: wlcg.cloud.cnaf.infn.it
Authorization: Basic z52…
Accept: */*
Content-Length: ...
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:token-exchange
&subject_token=eYnj…HvB6hykK
&audience=https%3A%2F%2Fse1.example%20https%3A%2F%2Fse2.example
&scope=openid%20wlcg.groups%20offline_access
```

5. FTS uses the token to drive a third-party transfer across the two storage
   elements

```
COPY /example/file HTTP/2
Host: se2.example
Source: https://se1.example/example/file
Authorization: Bearer e7nd…
TransferHeaderAuthorization: Bearer e7nd…
```
