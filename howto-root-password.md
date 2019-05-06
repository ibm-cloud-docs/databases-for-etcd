---
copyright:
  years: 2017,2018
lastupdated: "2018-11-08"

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Setting the Root Password
{: #root-password}

The {{site.data.keyword.databases-for-etcd_full}} service is provisioned with a root user.

You have to set the root password before you can use it to connect. To set the password through the {{site.data.keyword.cloud_notm}} dashboard, select _Manage_ from the service dashboard to open the management panel for your service. Open the _Settings_ tab, and use the _Change Password_ panel to set a new root password.

![The Root Passwor Panel in _Settings_](images/settings-root-password.png)

## Setting the root password from the command line

Use the `cdb user-password` command from the [{{site.data.keyword.databases-for}} CLI Plug-in](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference) to set the root password with the command line.

For example, to set the root password for a deployment named "example-deployment", use the following command.
```
ibmcloud cdb user-password example-deployment root <newpassword>
```

## Setting the root password from the API

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/deployments/{id}/users/{username}` endpoint to set the root password.

```
curl -X PATCH `https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users/root' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"password":"newrootpasswordsupersecure21"}'
```

For more information, see the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#set-database-level-user-s-password).

