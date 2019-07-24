---

Copyright:
  years: 2018, 2019
lastupdated: "2019-04-22"

keywords: etcd

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability and Performance
{: #high-availability}

{{site.data.keyword.databases-for-etcd_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-etcd}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with three nodes. All three nodes store data and maintain cluster state with a quorum. The consensus algorithm monitors cluster health, triggers failovers if one member becomes unhealthy, and accepts a new or rejoining member. If one data member becomes unreachable, your cluster continues to operate normally. If there is a permanent loss of a quorum, then the cluster is restarted and recovers to a previous state. 

## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-etcd}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruptions are not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods of no connectivity longer than a minute, so we can investigate.

## Performance 

{{site.data.keyword.databases-for-etcd}} deployments can be [scaled to your usage](/docs/services/databases-for-etcd?topic=databases-for-etcd-resources-scaling), but they do not auto-scale. There are a few factors to consider if you are concerned about the performance of your deployment.

### Storage Limits and Disk IOPS

etcd has a [storage limit](https://coreos.com/etcd/docs/latest/dev-guide/limit.html) of 8 GB of data on a single deployment. It is not possible to store more than 8 GB of data in an etcd deployment, and exceeding the limit will make your deployment read-only.

{{site.data.keyword.databases-for-etcd}} deployments over-allocate space because disk allocation affects the performance of the disk. Larger disks have higher performance with baseline Input-Output Operations per second (IOPS) of 10 IOPS for each GB. etcd relies on writing data to disk to maintain its consensus algorithm, so exceeding your IOPS deployment's limits can cause cluster instability. Scaling your deployment's disk resources increases the IOPS available to your deployment.

### Memory Usage

etcd uses memory to cache data and can benefit from increasing the amount of memory to at least the size of the data set. Memory is also used to maintain the watchers, so if your use-case requires thousands of watchers, you can benefit from increasing the amount of RAM available to your deployment ever further.

### Monitoring your deployment

Deployment owners can [monitor](/docs/services/databases-for-etcd?topic=databases-for-etcd-monitoring) the state of the deployment, estimate typical resource usage, and scale the deployment accordingly.

If you are planning on running operations that might put a spike in the usual resource usage, you can manually scale your service's resources up to avoid hitting resource limits.

## SLA

{{site.data.keyword.databases-for-etcd}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](https://cloud.ibm.com/docs/overview/zero_downtime.html#SLAs).