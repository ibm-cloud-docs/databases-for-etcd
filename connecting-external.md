---
copyright:
  years: 2019,2018
lastupdated: "2019-04-10"

keywords: etcd, application

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #external-app}

Each {{site.data.keyword.databases-for-etcd_full}} deployment has connection strings specifically for drivers and applications. Connection strings are displayed in the _Connections_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the users on your deployment. While you can use the root user for all of your connections and applications, it might be better to generate users specifically for your applications to connect with. Documentation on creating users is on the [Creating Users and Getting Connection Strings](/docs/databases-for-etcd?topic=databases-for-etcd-connection-strings) page.

## Using Connection Information

{{site.data.keyword.databases-for-etcd}} only supports the etcd v3 API and datastore. Access to the v2 API is disabled. 
{: .tip}

The information an application needs to make a connection to your deployment is in the "etcd" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for etcd, it is "uri"
`Scheme`||Scheme for a URI - for etcd, it is "https"
`Path`||Path for a uri
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver.
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, and Path
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. `etcd`/`URI` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## Connecting with a Driver

etcd drivers are often able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example, 
```
https://ibm_cloud_59699685_b95e_4afe_9d39_7464c228563c:$PASSWORD@ca537b4d-dcf2-467f-bd98-97535f11445b.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:32218
```

The table covers a few of the etcd drivers in various languages.

Language|Driver|Documentation
----------|-----------
Node|`etcd3`|[Link](https://mixer.github.io/etcd3/classes/index_.etcd3.html)
Java|`jetcd`|[Link](https://github.com/etcd-io/jetcd)
Java|`etcd-java`|[Link](https://github.com/IBM/etcd-java)
Go|`etcd/client`|[Link](https://github.com/etcd-io/etcd/tree/master/client)
Python|`python-etcd`|[Link](https://python-etcd.readthedocs.io/en/latest/)
{: caption="Table 2. Common etcd drivers" caption-side="top"}

## Connecting without a Driver

For languages that do not have gRPC support etcd v3 provides a JSON gRPC gateway that translates HTTP/JSON requests into gRPC messages. For example, you can check the cluster health by using cURL.
```
curl https://35dae549-2275-4d3e-beed-d86f36022336.974550db55eb4ec0983f023940bf637f.databases.appdomain.cloud:32460/{version}/cluster/member/list --cacert c5f02736-d94c-11e8-a2e9-62ec2ed68f84 \
-X POST -d '{"name": "ibm_cloud_59699685_b95e_4afe_9d39_7464c228563c", "password": "$PASSWORD"}'
```

The `version` path parameter depends on the minor version of etcd running on your deployment. You can find the minor version on your [deployment's _Overview_ page](/docs/databases-for-etcd?topic=databases-for-etcd-dashboard-overview). If you are running etcd 3.2, use `v3alpha` in the endpoint. If you are running etcd 3.3, use `v3beta` in the endpoint. Version information and example commands are in the [etcd documentation](https://github.com/etcd-io/etcd/blob/master/Documentation/dev-guide/api_grpc_gateway.md). Refer to the [etcd Swagger API definitions](https://github.com/etcd-io/etcd/blob/master/Documentation/dev-guide/apispec/swagger/rpc.swagger.json) for a full reference. 

## TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-etcd}} are TLS 1.2 enabled, so the method you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate to verify the server upon connection. 

### Using the self-signed certificate

1. Copy the certificate information from the _Connections_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

### CLI plug-in support for the self-signed certificate

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver or client.