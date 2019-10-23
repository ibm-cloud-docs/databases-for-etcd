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



# Performance
{: #performance}

{{site.data.keyword.databases-for-etcd}} deployments can be [scaled to your usage](/docs/services/databases-for-etcd?topic=databases-for-etcd-resources-scaling), but they do not auto-scale. There are a few factors to consider if you are concerned about the performance of your deployment.

## Storage Limits and Disk IOPS

etcd has a [storage limit](https://coreos.com/etcd/docs/latest/dev-guide/limit.html) of 8 GB of data on a single deployment. It is not possible to store more than 8 GB of data in an etcd deployment, and exceeding the limit will make your deployment read-only.

{{site.data.keyword.databases-for-etcd}} deployments over-allocate space because disk allocation affects the performance of the disk. Larger disks have higher performance with baseline Input-Output Operations per second (IOPS) of 10 IOPS for each GB. etcd relies on writing data to disk to maintain its consensus algorithm, so exceeding your IOPS deployment's limits can cause cluster instability. Scaling your deployment's disk resources increases the IOPS available to your deployment.

## Memory Usage

etcd uses memory to cache data and can benefit from increasing the amount of memory to at least the size of the data set. Memory is also used to maintain the watchers, so if your use-case requires thousands of watchers, you can benefit from increasing the amount of RAM available to your deployment ever further.

## Monitoring your deployment

Deployment owners can [monitor](/docs/services/databases-for-etcd?topic=cloud-databases-monitoring) the state of the deployment, estimate typical resource usage, and scale the deployment accordingly.

If you are planning on running operations that might put a spike in the usual resource usage, you can manually scale your service's resources up to avoid hitting resource limits.