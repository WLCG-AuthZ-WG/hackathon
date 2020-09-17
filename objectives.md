# Objectives

## IAM

- Improved client registration management
  - Limited to VO members
  - Improved UI

- Cleanup of unused refresh tokens after a configurable period (to avoid pile
  up of tokens in the database)

- Cleanup of unused clients after a configurable period (to avoid pile up of
  unused clients)

## RUCIO

- Audience support
- Audience request support (to request tokens with the 'FTS' audience)
- Group-based authorization support
- Support for [Bearer token discovery][bearer-token-discovery] on the CLI

## GFAL

- Support for [Bearer token discovery][bearer-token-discovery]
- Support for token-based authz on HTTP TURLs in SRM prepareToGet handling

## Davix 

- Support for [Bearer token discovery][bearer-token-discovery]

## FTS

- Audience support
- Audience request support (to request tokens targeted to specific transfer
  endpoints)
- Fine grained scope request support
  - Request the minimum scopes to manage a transfer
- Group-based authorization support
- Support for [Bearer token discovery][bearer-token-discovery] on the CLI

## dCache

- Audience support
- Fine-grained scope-based authorization support
- WLCG group-based authorization support
- Support for [Bearer token discovery][bearer-token-discovery] on the CLI

## XRooTD

- Audience support
- Fine-grained scope-based authorization support
- WLCG group-based authorization support

## EOS

- Audience support
- Fine-grained scope-based authorization support
- WLCG group-based authorization support

## DPM

- Audience support
- Fine-grained scope-based authorization support
- WLCG group-based authorization support

## StoRM

- Audience support
- Fine-grained scope-based authorization support
- WLCG group-based authorization support

## Other

[bearer-token-discovery]: https://github.com/WLCG-AuthZ-WG/bearer-token-discovery
