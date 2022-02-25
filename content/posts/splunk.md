---
title: "Splunk"
date: 2022-02-20T15:05:20+08:00
summary: Documentation on how to manage splunk
tags:
  - splunk
categories:
  - splunk
author: Jerome
---

## Splunk Configurations

### server.conf

$SPLUNK_HOME/etc/system/local/server.conf

Related documentations

* https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/ConfigureS2Sonnewcipher

``` ini
[node_auth]
signatureVersion = v2

[general]
legacyCiphers = disabled

[replication_port-ssl://8080]
serverCert =
sslPassword = 
requireClientCert = true
useSSLCompression = true

[sslConfig]
sslRootCAPath = 
requireClientCert = true

[kvstore]
storageEngine = wiredTiger
serverCert = 

```

### inputs.conf

Related documentations

* https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/ConfigureSplunkforwardingtousesignedcertificates

``` ini
```

### web.conf

$SPLUNK_HOME/etc/system/local/web.conf
``` ini
[settings]
enableSplunkWebSSL = true
privKeyPath = 
serverCert = 
```

### authentication.conf

$SPLUNK_HOME/etc/system/local/authentication.conf

``` ini
[authentication]
authType = SAML
authSettings = saml

[roleMap_saml]


```

## Rotate splunk.secret

### single instance

cli

``` shell
splunk rotate splunk-secret
```

rest

``` shell
curl -u <splunk username>:<splunk password> https://<splunk server>:<management port>/services/server/security/splunk-secret/rotate -X POST
```

### search head cluster

cli

``` shell
splunk rotate shcluster-splunk-secret
```

rest

``` shell
curl -u <splunk username>:<splunk password> https://<splunk server>:<management port>/services/shcluster/captain/control/control/rotate-splunk-secret -X POST
```


## Data flow table

https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/AboutsecuringyourSplunkconfigurationwithSSL#Ways_you_can_secure_Splunk_Enterprise

## Decrypt pass4SymmKey

unencrypted cannot starts with `$7$`

``` shell
splunk show-decrypted --value ‘pass4SymmKeyGoesHere’
```

## Manage admin password

password cannot starts with `$6$`

https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/Secureyouradminaccount

create admin user account with `$SPLUNK_HOME/etc/system/local/user-seed.conf`

get hashed password using command `bin/splunk hash-passwd $PLAIN_PASSWORD`

``` ini
[user_info]
USERNAME = admin
HASHED_PASSWORD = 
```


## TLS encryption and cipher suites

https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/AboutTLSencryptionandciphersuites

all use `tls1.2`

## Important naming guidelines when creating users and roles

When you create users and roles within the native authentication scheme, note the following caveats:

1. Usernames stored in the native authentication scheme cannot contain spaces, colons, or forward slashes.
2. Usernames are not case-sensitive. For example: Jacque, jacque, and JacQue are all the same to the native Splunk authentication scheme.
3. Role names must use lowercase characters only. They cannot contain spaces, colons, or forward slashes.

https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/Setupbuilt-inauthentication#Important_naming_guidelines_when_creating_users_and_roles

## Disable automatic chart recovery

https://docs.splunk.com/Documentation/Splunk/8.2.5/Analytics/Charts#Recover_workspace_charts


edit `$SPLUNK_HOME/etc/apps/splunk_metrics_workspace/src/main/resources/splunk/default`, In the features stanza, add the following line: `state_restore = 0`.

## Configure network ACLs in Splunk Enterprise

https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/Useaccesscontrollists

## Configure password policy

https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/Configurepasswordsinspecfile


## Secure SSO with TLS certificates on Splunk Enterprise

http://docs.splunk.com/Documentation/Splunk/8.2.5/Security/ConfigureSSLforSSO
