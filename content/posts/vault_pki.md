---
title: "Hashicorp Vault PKI"
date: 2022-06-30T18:59:41+08:00
summary: Documentation on how to manage splunk
tags:
  - hashicorp vault
  - pki
categories:
  - hashicorp vault
  - pki
author: Jerome
---

## Setup


### Start Vault

```
vault server -dev -dev-root-token-id=root -dev-listen-address=$HOSTNAME:8200
```


### Setup Client

```
export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=root
```


### Setup Issue CA and sign certificate

```
vault secrets enable pki
vault secrets tune -max-lease-ttl=87600h pki
vault write -field=certificate pki/root/generate/internal \
     common_name="ec2.internal" \
     issuer_name="root-2022" \
     ttl=87600h > root_2022_ca.crt

vault write pki/config/urls \
     issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
     crl_distribution_points="$VAULT_ADDR/v1/pki/crl"


vault secrets enable -path=pki_int pki
vault secrets tune -max-lease-ttl=43800h pki_int

vault write -format=json pki_int/intermediate/generate/internal \
     common_name="ec2.internal Intermediate Authority" \
     issuer_name="ec2-dot-internal-intermediate" \
     | jq -r '.data.csr' > pki_intermediate.csr

vault write -format=json pki/root/sign-intermediate \
     issuer_ref="root-2022" \
     csr=@pki_intermediate.csr \
     format=pem_bundle ttl="43800h" \
     | jq -r '.data.certificate' > intermediate.cert.pem

vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem

vault write pki_int/roles/ec2-dot-internal \
     issuer_ref="$(vault read -field=default pki_int/config/issuers)" \
     allowed_domains="ec2.internal" \
     allow_subdomains=true \
     max_ttl="2160h"

vault write pki_int/issue/ec2-dot-internal common_name="test.ec2.internal" ttl="2160h"
```

