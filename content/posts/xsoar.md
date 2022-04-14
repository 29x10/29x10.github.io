---
title: "XSOAR"
date: 2022-04-11T23:10:00+08:00
summary: Documentation on how to manage xsoar
tags:
  - xsoar
categories:
  - xsoar
author: Jerome
---

## XSOAR

### License

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/cortex-xsoar-overview/cortex-xsoar-licenses/add-a-license

For customers using a custom location, the path can be found in the **license.file.path** configuration key in the demisto configuration file. The default location of the configuration file is **/etc/demisto.conf**.

### Refresh users in use number

1. Go to **Settings > About > License**.
2. In the **Users in use** section, click **Refresh**.

### Disable Telemetry

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/cortex-xsoar-overview/cortex-xsoar-telemetry

To disable telemetry, go to **Settings > About > Troubleshooting > Telemetry**.

### Keyboard Shortcuts

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/cortex-xsoar-overview/keyboard-shortcuts

### Installer flags

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/installation/install-demisto-on-a-physical-or-virtual-server/installer-flags

### Failure notification

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/incidents/incident-management/receive-notification-on-an-incident-fetch-error

### Certificate

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/installation/post-installation-checklist/https-with-a-signed-certificate

### Websocket buffer

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/installation/post-installation-checklist/websocket-configuration

### Proxy

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/proxy/configure-proxy-settings#id90a5a3e1-2ffd-4a25-812d-54f8f6eb1cbe

### Elasticsearch

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/elasticsearch/elasticsearch-setup/best-practices

#### Server Spec

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/elasticsearch/elasticsearch-setup/elasticsearch-system-requirements

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/high-availability/sizing-requirements-for-high-availability-deployments


#### Configurations

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/elasticsearch/elasticsearch-setup/elasticsearch-configurations

#### Required permissions

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/elasticsearch/elasticsearch-security/elasticsearch-security-guidelines-for-single-instance-deployments

#### Archive Data

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/elasticsearch/archive-data-with-elasticsearch

#### Roles

#### Default Administrator

Default administrators are usually used for troubleshooting, they are not counted as license users, cannot be deleted, and are also tenant administrators.

set `incident.restrict.default.admin` property to `true`

#### RBAC

For example, if you want to enable chat in the War Room, but exclude permissions for everything else, you should give the role Read/Write permissions under Data but remove all other granular data permissions. Also remove permissions in Integrations (under Settings). This leaves the role with access to chat only.

#### Self-Service Read-Only Users

`create.read.only.users` and value `true`.

Enter the key `dashboards.read.only.users` and type a comma-separated list of the names of the dashboards you created for the self-service read-only users.

### Load Balance

The load balancer must be configured to use `sticky sessions` and `request timeouts`. The load balancer should use a health check to verify that the App Server is up and running. Cortex XSOAR recommends you use the `/health` route and verify that you receive a 200 HTTP response.

#### Upate engine url

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/high-availability/deploy-engines-in-a-high-availability-environment#idc701334f-8f2d-40ef-b724-3729f4c6380d

### Adding new app server

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/high-availability/install-additional-app-servers

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/high-availability/install-additional-app-servers/validate-additional-app-servers

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/high-availability/deploy-engines-in-a-high-availability-environment

### Offline marketplace

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/marketplace/install-a-content-pack-offline/configure-the-marketplace-for-offline-installation


| KEY | VALUE |
|---|---|
|marketplace.sync.enabled|false|
|marketplace.content.packs.rating.sync.enabled|false|
|marketplace.subscriptions.sync.enabled|false|


### Podman

#### Selinux

https://docs.paloaltonetworks.com/cortex/cortex-xsoar/6-6/cortex-xsoar-admin/podman/configure-the-selinux-policy-for-podman








