---

Copyright:
  years: 2018, 2019
lastupdated: "2019-09-12"

keywords: etcd

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-etcd_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} environment. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-etcd}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with three nodes. All three nodes store data and maintain cluster state with a quorum. The consensus algorithm monitors cluster health, triggers failovers if one member becomes unhealthy, and accepts a new or rejoining member. If one data member becomes unreachable, your cluster continues to operate normally. If there is permanent loss of a quorum, then the cluster is restarted and recovers to a previous state. 

## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-etcd}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable.

Your applications must be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruptions are not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods of no connectivity longer than a minute, so we can investigate.

## High availability, disaster recovery, and SLA resources

{{site.data.keyword.databases-for-etcd}} deployments conform to the {{site.data.keyword.cloud_notm}} Databases [HA, DR, and SLA](/docs/cloud-databases?topic=cloud-databases-ha-dr) information and terms.

