# Second Token-based AuthN/Z Hackathon

- When: 16-17-18 September 2020
- Agenda: https://indico.cern.ch/e/tb-hackathon

## Topics under discussion

- [WLCG JWT profile compliance for DOMA TPC](./wlcg-jwt-profile.md)
  - Audience restriction
  - Scope-based authorization
  - Group-based authorization
- [Local user mapping](./local-user-mapping.md)

## Pre-requisites

- All devs registered in the [WLCG VO][wlcg-vo]
- WLCG VO trusted at all services

The WLCG storage area authorization at SEs configured as follows:

- WLCG JWT profile capability-based authorization enabled
  - AuthZ will be based on the `storage.*` scopes in the token
- Read-only access (i.e., the ability to list directory contents and
  read files) to all members of the WLCG VO, i.e.:
  - all clients presenting a valid VOMS proxy for the WLCG VO
  - all clients presenting a valid JWT token issued by the WLCG token issuer
- Write-access to the `/protected` folder (and sub-folders) is granted to the following principals:
  - all clients presenting a valid VOMS proxy with the `/wlcg/Role=test` role
  - all clients presenting a valid JWT token with the `/wlcg/test` group

## Hackathon objectives

- Fully token-based transfers managed by RUCIO with scope and group-based
  authorization
- Audience restrictions honoured at all services
- Development of a first incarnation of TPC integration smoke test suite, based
  on Robot Framework
- Local user mapping proposal

[wlcg-vo]: http://https://wlcg.cloud.cnaf.infn.it
