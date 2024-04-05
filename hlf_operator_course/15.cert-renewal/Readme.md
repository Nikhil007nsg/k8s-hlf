# Automatic renewal of certificates

## Parameters

The following command line parameters are available for the operator:

- `auto-renew-orderer-certificates-delta`
- `auto-renew-peer-certificates-delta`
- `auto-renew-peer-certificates`
- `auto-renew-orderer-certificates`

## Orderer Certificate Renewal

The operator supports the auto renewal of certificates for orderers. The following command line parameters are available for the operator:

- `auto-renew-orderer-certificates-delta`
- `auto-renew-orderer-certificates`

The `auto-renew-orderer-certificates-delta` parameter specifies the number of days before the expiration of the orderer certificates when the operator should start the renewal process. The default value is 15 days.

The `auto-renew-orderer-certificates` parameter enables the auto renewal of certificates for orderers. The default value is false.

## Peer Certificate Renewal

The operator supports the auto renewal of certificates for peers. The following command line parameters are available for the operator:

- `auto-renew-peer-certificates-delta`
- `auto-renew-peer-certificates`

The `auto-renew-peer-certificates-delta` parameter specifies the number of days before the expiration of the peer certificates when the operator should start the renewal process. The default value is 15 days.

The `auto-renew-peer-certificates` parameter enables the auto renewal of certificates for peers. The default value is false.


## Setup Auto renewal

```bash
helm upgrade --install hlf-operator --version=1.9.2 -- kfs/hlf-operator -f values.yaml
```
