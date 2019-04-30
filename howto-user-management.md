---
copyright:
  years: 2019
lastupdated: "2019-04-24"

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Managing Users and Roles
{: #user-management}

{{site.data.keyword.databases-for-etcd_full}} deployments come with authentication enabled and use etcd's [built-in access control](https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/authentication.md). 

While technically possible - turning off etcd authentication is **NOT** a good idea and highly discouraged. {{site.data.keyword.databases-for-etcd}} automatically fixes any deployments where etcd authentication has been deliberately turned off.
{: .tip}

When you provision a new deployment in {{site.data.keyword.cloud_notm}}, you are automatically given access to the etcd root user. You can also add users in the _Service Credentials_ panel, which allows for access to etcd to be integrated with your {{site.data.keyword.cloud_notm}} account and [IAM](/docs/services/databases-for-etcd?topic=cloud-databases-iam).

## The root user

The [etcd root user](https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/authentication.md#special-users-and-roles) is intended for use as an administrative user. It is granted the root role, which gives read/write access to the entire keyspace, cluster operations, and privileges to create, modify, and delete other roles and users on your deployment.

## etcd roles

etcd uses a system of roles to manage database and key access. Roles are used to give users a set of privileges. If you connect to your deployment using the root user and [`ectdctl`](/docs/services/databases-for-etcd?topic=databases-for-etcd-connecting-etcdctl), you can check the roles on your deployment.
```
ETCDCTL_API=3 etcdctl --cacert=c5f07736-d94c-11e8-a2e9-62ec2ed68f84 --endpoints=https://35dae549-2275-4d3e-baed-d86f36022336.974550db65eb4ec0983f023940bf637f.databases.appdomain.cloud:32460 --user=root:$PASSWORD role list
```

You can also create roles that grant read access, write access, or both to specific keys or ranges of keys. Refer to the etcd documentation for [commands to manage roles](https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/authentication.md#working-with-roles).

Once you have created a role, you can assign it to a user. You can list the users on your deployment with
```
ETCDCTL_API=3 etcdctl --cacert=c5f07736-d94c-11e8-a2e9-62ec2ed68f84 --endpoints=https://35dae549-2275-4d3e-baed-d86f36022336.974550db65eb4ec0983f023940bf637f.databases.appdomain.cloud:32460 --user=root:$PASSWORD user list
```

## _Service Credential_ Users

Users that you [create through the _Service Credentials_ panel](/docs/services/databases-for-etcd?topic=databases-for-etcd-connection-strings#generating-connection-strings-from-service-credentials) are given the role `rwall`. They have access read and write access to all the keys in the database.

If you need users that are created from _Service Credentials_ to have a different role, you can use the root user to change their role.

## Users created through the CLI and the API

Users that are created in the [CLI](/docs/services/databases-for-etcd?topic=databases-for-etcd-connection-strings#getting-credentials-and-connection-strings-from-the-command-line) and [API](/docs/services/databases-for-etcd?topic=databases-for-etcd-connection-strings#getting-credentials-and-connection-strings-with-the-api) get the same role as _Service Credential_ users, `rwall`. They have access read and write access to all the keys in the database. If you need them to have a different role that you have created, you can use the root user to change their role.

Users that are created directly from the API and CLI do not appear in _Service Credentials_, but you can [add them](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-connection-strings#generating-service-credentials-for-existing-users) if you choose.

## Users created in etcd

You can bypass creating users in _Service Credentials_, the CLI, the API, and [create users directly in etcd](https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/authentication.md#working-with-users). 

Users created directly in etcd do not appear in _Service Credentials_, but you can [add them there](/docs/services/databases-for-etcd?topic=databases-for-etcd-connection-strings#generating-service-credentials-for-existing-users) if you choose. Note, that these users are not integrated with IAM controls, even if added to _Service Credentials_.

## The compose user

If you use `etcdctl` to list the users on your deployment, you might have noticed a user that is named `compose`. The `compose` user is the internal administrative account that manages replication, cluster operations, and other functions that ensure the stability of your deployment. It has the same roles and privileges as the root user. Changing or deleting to the `compose` user is not advised and will disrupt the stability of your deployment.