---
title: openssl
date: 2022-01-29T21:10:52+08:00
summary: Documentation on how to handle certificate
tags:
  - openssl
  - certificate
  - SSL
  - mTLS
categories:
  - openssl
author: Jerome
---

# OpenSSL

## Documentation

1. https://www.openssl.org/docs/man1.1.1/man1/openssl-rsa.html
2. https://www.openssl.org/docs/man1.1.1/man1/openssl-req.html
3. https://www.openssl.org/docs/man1.1.1/man5/x509v3_config.html
4. https://www.openssl.org/docs/man1.1.1/man1/openssl-genpkey.html
5. https://www.openssl.org/docs/man1.1.1/man1/openssl-x509.html

---

## Private key

### PKCS#1

When using `openssl genrsa` command, the private key are in **PKCS#1** format

Example of unencrypted by `openssl genrsa 2048`

```
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

Example of encrypted by `openssl genrsa -aes256 -passout pass:password 2048`

```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-256-CBC,F8C9D9D84DC90042F39580997E1FECE6

...
-----END RSA PRIVATE KEY-----
```

---

### PKCS#8

When using `openssl genpkey` command, the private key are in *PKCS#8* format

Example of unencrypted by `openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048`

```
-----BEGIN PRIVATE KEY-----
...
-----END PRIVATE KEY-----
```

Example of encrypted by `openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -aes256 -pass pass:password`

```
-----BEGIN ENCRYPTED PRIVATE KEY-----
...
-----END ENCRYPTED PRIVATE KEY-----
```

---

### How to convert

#### **PKCS#1** -> **PKCS#8**

Unencrypted in **PKCS#1** and unencrypted in **PKCS#8**

``` shell
openssl pkcs8 -in private_pkcs1.pem -topk8 \
-nocrypt
```

Unencrypted in **PKCS#1** and encrypted in **PKCS#8**

``` shell
openssl pkcs8 -in private_pkcs1.pem -topk8 \
-passout pass:password
```

Encrypted in **PKCS#1**, but unencrypted in **PKCS#8**

``` shell
openssl pkcs8 -in private_pkcs1.pem -topk8 \
-passin pass:password -nocrypt
```

Encrypted in **PKCS#1**, and encrypted in **PKCS#8**

``` shell
openssl pkcs8 -in private_pkcs1.pem -topk8 \
-passin pass:password -passout pass:new_password
```

---

#### **PKCS#8** -> **PKCS#1**

Unencrypted in **PKCS#8** and unencrypted in **PKCS#1**

``` shell
openssl rsa -in private_pkcs8.pem
```

Unencrypted in **PKCS#8** but encrypted in **PKCS#1**

``` shell
openssl rsa -in private_pkcs8.pem \
-aes256 -passout pass:password
```

Encrypted in **PKCS#8** but unencrypted in **PKCS#1**

``` shell
openssl rsa -in private_pkcs8.pem \
-passin pass:password
```

Encrypted in **PKCS#8** and encrypted in **PKCS#1**

``` shell
openssl rsa -in private_pkcs8.pem \
-passin pass:password -aes256 -passout pass:new_password
```

---

### How to check

validate password and check bits

```
openssl rsa -passin pass:password -text -noout -in key.pem
```

checking encryption cipher of **PKCS#8**

```
openssl asn1parse -in encrypted.pkcs8.pem
```

---

## CSR configuration

Configuration documentation can be found [here](https://www.openssl.org/docs/man1.1.1/man1/openssl-req.html#CONFIGURATION-FILE-FORMAT)

Available subject alternative name can be found [here](https://www.openssl.org/docs/man1.1.1/man5/x509v3_config.html#Subject-Alternative-Name)
```
[req]
default_bits = 2048
default_keyfile = key.pem
distinguished_name = req_distinguished_name
prompt = no
req_extensions = v3_req

[req_distinguished_name]
C = CN
ST = Province
L = City
O = Organization
OU = Organization Unit Name
CN = Common Name
emailAddress = contact@domain

[v3_req]
subjectAltName = @alt_section

[alt_section]
DNS.1 = HOST_1_FQDN
DNS.2 = LB_FQDN
IP.1 = 192.168.1.1
```

---

## Certificate Signing Request (CSR)

### Create

Private key encrypted

``` shell
openssl req -new -config csrconfig.conf -out server.csr -passout pass:password
```

Private key unencrypted

``` shell
openssl req -new -config csrconfig.conf -out server.csr -nodes
```

---

### Validate

```shell
openssl req -in server.csr -text -noout
```

---

## Create CA

```
openssl genrsa -out rootCA.key 2048

openssl req -x509  -new -nodes \
-key rootCA.key \
-days 3650 \
-out rootCA.pem \
-subj "/C=CN/ST=Province/L=City/O=TestOrg/OU=TestBU/CN=TestCA"
```

---

## Sign certificate by CA

```
openssl x509 -req \
 -in server.csr \
 -extensions server_ext \
 -CA rootCA.pem \
 -CAkey rootCA.key \
 -CAcreateserial \
 -out server.crt \
 -days 365 \
 -extfile \
 <(echo "[server_ext]"; echo "extendedKeyUsage=serverAuth,clientAuth"; echo "subjectAltName=DNS:HOST_1_FQDN,DNS:LB_FQDN,IP:192.168.1.1")
```
