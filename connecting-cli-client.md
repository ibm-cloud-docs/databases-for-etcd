---
copyright:
  years: 2018,2019
lastupdated: "2019-04-10"

keywords: etcd, etcdctl

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Connecting with `etcdctl`
{: #connecting-etcdctl}

You can access your etcd database directly from a command-line client, which allows for direct interaction with the data structures that are created within the database. It is also useful for administering and monitoring the keyspace and performance, making etcd transactions, managing leases, and other management activities.

{{site.data.keyword.databases-for-etcd}} only supports the etcd v3 API and datastore. Access to the v2 API is disabled. 
{: .tip}

## Installing 

The `etcdctl` binary is available in the etcd distribution, which can be downloaded from [the coreos/etcd repository](https://github.com/coreos/etcd/releases/latest).

## Connecting

Connection strings are displayed in the _Connections_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

Any user on your deployment is able to connect using `etcdctl`, but the [root user](/docs/databases-for-etcd?topic=databases-for-etcd-user-management#the-root-user) does have more permissions on the cluster.
{: .tip}

The information `etcdctl` needs to make a connection to your deployment is in the "cli" section of your [connection strings](/docs/databases-for-etcd?topic=databases-for-etcd-connection-strings). The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Bin`||The recommended binary to create a connection; in this case it is `etcdctl`.
`Composed`||A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command-line parameters.
`Environment`||A list of key/values you set as environment variables.
`Arguments`|0...|The information that is passed as arguments to the command shown in the Bin field.
`Certificate`|Base64|A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded.
`Certificate`|Name|The allocated name for the self-signed certificate.
`Type`||The type of package that uses this connection information; in this case `cli`. 
{: caption="Table 2. `etcdctl`/`cli` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## `etcdctl` example

```
ETCDCTL_API=3 etcdctl --endpoints=http://afe6f1d5-60d5-447e-a96a-66f555ecc277.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:32207 --user=ibm_cloud_4417:32f81b04e1b756f34bda351d59c973 member list -w table
```

* `ETCDCTL_API=3` - Sets the API version environment variable for the `etcdctl` command. The binary for `etcdctl` uses version 2 by default, which is not supported on your deployment. Setting this environment variable overrides the default. If you are only using `etcdctl` to talk to etcd v3 deployments, you might want to set this variable more permanently in your shell environment.
* `etcdctl` - The command itself. 
* `--endpoints=...` - The parameter that specifies the endpoints where the `etcdctl` command connects. It's composed of HTTPS protocol URLs and includes a port number. 
* `--user=...` - The parameter for the username and password, separated by a colon, to be used as credentials to log in to the etcd deployment. 
* `member list` - An etcdctl command to list the database members of the etcd deployment. Without any other parameters, the result is produced as a list comma separated values.
* `-w table` - A modifier for the output of `member list`, which reformats it as a table with headings.

More example commands are in the [etcd documentation](https://github.com/etcd-io/etcd/blob/master/Documentation/dev-guide/interacting_v3.md).

## Starting `etcdctl` from the IBM Cloud CLI

If you have both `etcdctl` and the [{{site.data.keyword.databases-for}} CLI Plug-in](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference) installed, the `ibmcloud cdb deployment-connections` command can handle creating the connection. For example, to connect to a deployment named  "example-etcd" with an "example-user", use the following command.

```
ibmcloud cdb deployment-connections -u example-user example-etcd --start
```

The command prompts for the user's password and then runs the `etcdctl` command-line client to connect to the database.