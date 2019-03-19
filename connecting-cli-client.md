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


# Connecting with `etcdctl`
{: #connecting-etcdctl}

You can access your etcd database directly from a command-line client, which allows for direct interaction and monitoring of the data structures that are created within the database. It is also useful for administering and monitoring the keyspace and performance, making etcd transactions, managing leases, and other management activities.

## Installing 

The `etcdctl` binary is available in the etcd distribution, which can be downloaded from [the coreos/etcd repository](https://github.com/coreos/etcd/releases/latest).

## Connecting

The information `etcdctl` needs to make a connection to your deployment is in the "cli" section of your [connection strings](/docs/services/databases-for-etcd?topic=databases-for-etcd-connection-strings). The table contains a breakdown for reference.

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

* `ETCDCTL_API=3` - Sets the API version environment variable for the `etcdctl` command. The binary for `etcdctl` supports both version 2 and 3 of the API. By default, it uses version 2. Setting this environment variable overrides the default. If you are only using `etcdctl` to talk to etcd v3 deployments, you might want to set this variable more permanently in your shell environment.
* `etcdctl` - The command itself. 
* `--endpoints=...` - The parameter that specifies the endpoints where the `etcdctl` command connects. It's composed of HTTPS protocol URLs and includes a port number. 
* `--user=...` - The parameter for the username and password, separated by a colon, to be used as credentials to log in to the etcd deployment. 
* `member list` - An etcdctl command to list the database members of the etcd deployment. Without any other parameters, the result is produced as a list comma separated values.
* `-w table` - A modifier for the output of `member list`, which reformats it as a table with headings.

## Starting `etcdctl` from the IBM Cloud CLI

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating the client connection. For example, to connect to a deployment named  "example-etcd" with an "example-user", use the following command.

```
ibmcloud cdb deployment-connections -u example-user admin example-etcd --start
```
Or
```
ibmcloud cdb cxn -u example-user example-etcd -s
```

The command prompts for the user's password and then runs the `etcdctl` command-line client to connect to the database.

If you don't specify a user, the `deployment-connections` commands return information for the root user by default.
{: .tip}