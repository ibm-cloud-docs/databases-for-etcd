---
copyright:
  years: 2019
lastupdated: "2022-02-02"

keywords: etcd, etcd manage users

subcollection: databases-for-etcd

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Managing Users and Roles
{: #user-management}

{{site.data.keyword.databases-for-etcd_full}} deployments come with authentication enabled and use etcd's [built-in access control](https://etcd.io/docs/v3.4.0/op-guide/authentication/). 

While technically possible - turning off etcd authentication is **NOT** a good idea and highly discouraged. {{site.data.keyword.databases-for-etcd}} automatically fixes any deployments where etcd authentication has been deliberately turned off.
{: .tip}

When you provision a new deployment in {{site.data.keyword.cloud_notm}}, you are automatically given access to the etcd root user. You can also add users in the _Service Credentials_ panel, which allows for access to etcd to be integrated with your {{site.data.keyword.cloud_notm}} account and [IAM](/docs/databases-for-etcd?topic=cloud-databases-iam).

## The root user
{: #user-management-root-user}

The [etcd root user](https://etcd.io/docs/v3.4.0/op-guide/authentication/#special-users-and-roles) is intended for use as an administrative user. It is granted the root role, which gives read/write access to the entire key space, cluster operations, and privileges to create, modify, and delete other roles and users on your deployment.

## etcd roles
{: #user-management-etcd-roles}

etcd uses a system of roles to manage database and key access. Roles are used to give users a set of privileges. If you connect to your deployment using the root user and [`ectdctl`](/docs/databases-for-etcd?topic=databases-for-etcd-connecting-etcdctl), you can check the roles on your deployment.
```sh
ETCDCTL_API=3 etcdctl --cacert=c5f07736-d94c-11e8-a2e9-62ec2ed68f84 --endpoints=https://35dae549-2275-4d3e-baed-d86f36022336.974550db65eb4ec0983f023940bf637f.databases.appdomain.cloud:32460 --user=root:$PASSWORD role list
```

You can also create roles that grant read access, write access, or both to specific keys or ranges of keys. Refer to the etcd documentation for [commands to manage roles](https://etcd.io/docs/v3.4.0/op-guide/authentication/#working-with-roles).

Once you have created a role, you can assign it to a user. You can list the users on your deployment with
```sh
ETCDCTL_API=3 etcdctl --cacert=c5f07736-d94c-11e8-a2e9-62ec2ed68f84 --endpoints=https://35dae549-2275-4d3e-baed-d86f36022336.974550db65eb4ec0983f023940bf637f.databases.appdomain.cloud:32460 --user=root:$PASSWORD user list
```

## _Service Credential_ Users
{: #user-management-service-cred-users}

Users that you create through the _Service Credentials_ panel are given the role `rwall`. They have access read and write access to all the keys in the database.

If you need users that are created from _Service Credentials_ to have a different role, you can use the root user to change their role.

## Users who are created through the CLI and the API
{: #user-management-cli-api-users}

Users that are created in the CLI and API get the same role as _Service Credential_ users, `rwall`. They have access read and write access to all the keys in the database. If you need them to have a different role that you have created, you can use the root user to change their role.

Users that are created directly from the API and CLI do not appear in _Service Credentials_, but you can add them if you choose.

## Users created in etcd
{: #user-management-etcd-users}

You can bypass creating users in _Service Credentials_, the CLI, the API, and [create users directly in etcd](https://etcd.io/docs/v3.4.0/op-guide/authentication/#working-with-users). 

Users who are created directly in etcd do not appear in _Service Credentials_, but you can [add them there](/docs/databases-for-etcd?topic=databases-for-etcd-connection-strings#adding-users-to-_service-credentials_)) if you choose.

## The compose user
{: #user-management-compose-user}

If you use `etcdctl` to list the users on your deployment, you might have noticed a user that is named `compose`. The `compose` user is the internal administrative account that manages replication, cluster operations, and other functions that ensure the stability of your deployment. It has the same roles and privileges as the root user. Changing or deleting to the `compose` user is not advised and will disrupt the stability of your deployment.
