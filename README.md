# Second Token-based AuthN/Z Hackathon

- When: 16-17-18 September 2020, 15-18 CEST
- Agenda: https://indico.cern.ch/e/tb-hackathon

## Topics under discussion

- [WLCG JWT profile compliance for DOMA TPC](./wlcg-jwt-profile.md)
  - Audience restriction
  - Scope-based authorization
  - Group-based authorization
- [Local user mapping](./local-user-mapping.md)
- [Bearer token discovery
  support](https://github.com/WLCG-AuthZ-WG/bearer-token-discovery/blob/master/specification.md)

## Pre-requisites

- All devs registered in the [WLCG VO][wlcg-vo]
- WLCG VO trusted at all services

The WLCG storage area authorization at SEs configured as follows:

- WLCG JWT profile capability-based authorization enabled
  - AuthZ will be based on the `storage.*` scopes in the token, e.g. a token
    with the `storage.modify:/` issued by the WLCG token issuer will grant
    write access on the whole storage area.
- Read-only access (i.e., the ability to list directory contents and
  read files) to all members of the WLCG VO, i.e.:
  - all clients presenting a valid VOMS proxy for the WLCG VO
  - all clients presenting a valid JWT token issued by the WLCG token issuer
- Write access (with the exclusion of the `/protected` folder) is granted
  to any client presenting a valid WLCG VO proxy
- Write-access to the `/protected` folder (and sub-folders) is granted to the following principals:
  - all clients presenting a valid VOMS proxy with the `/wlcg/Role=test` role
  - all clients presenting a valid JWT token with the `/wlcg/test` group

To enable JWT profile compliance testing, it would help to have a storage area
at each SE configured to trust the `https://tf.cloud.cnaf.infn.it` token
issuer.

## Hackathon objectives

- Fully token-based transfers managed by RUCIO with scope and group-based
  authorization
- Audience restrictions honoured at all services
- Development of a first incarnation of TPC integration smoke test suite, based
  on Robot Framework
- Local user mapping proposal

A fine-grained list of possible objectives, per component, is given
[here](./objectives.md).

[wlcg-vo]: https://wlcg.cloud.cnaf.infn.it
