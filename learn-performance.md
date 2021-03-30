---

Copyright:
  years: 2018, 20201
lastupdated: "2021-03-30"

keywords: etcd, databases, monitoring, scaling, autoscaling, resources

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# Performance
{: #performance}

{{site.data.keyword.databases-for-etcd_full}} deployments can be both manually [scaled to your usage](/docs/databases-for-etcd?topic=databases-for-etcd-resources-scaling), or configured to [autoscale](/docs/databases-for-etcd?topic=databases-for-etcd-autoscaling) under certain resource conditions. There are a few factors to consider if you are tuning the performance of your deployment.

## Monitoring your deployment

{{site.data.keyword.databases-for-etcd}} deployments offer an integration with the [{{site.data.keyword.monitoringfull}} service](/docs/databases-for-etcd?topic=databases-for-etcd-sysdig-monitoring) for basic monitoring of resource usage on your deployment. Many of the available metrics, like disk usage and IOPS, are presented to help you configure [autoscaling](/docs/databases-for-etcd?topic=databases-for-etcd-autoscaling) on your deployment. Observing trends in your usage and configuring the autoscaling to respond to them can help alleviate performance problems before your databases become unstable due to resource exhaustion.

## Storage Limits and Disk IOPS

etcd has a [storage limit](https://coreos.com/etcd/docs/latest/dev-guide/limit.html) of 8 GB of data on a single deployment. It is not possible to store more than 8 GB of data in an etcd deployment, and exceeding the limit will make your deployment read-only.

{{site.data.keyword.databases-for-etcd}} deployments over-allocate space because disk allocation affects the performance of the disk. Larger disks have higher performance with baseline input/output operations per second (IOPS) of 10 IOPS for each GB. etcd relies on writing data to disk to maintain its consensus algorithm, so exceeding your IOPS deployment's limits can cause cluster instability. Scaling your deployment's disk resources increases the IOPS available to your deployment.

## Memory Usage

etcd uses memory to cache data and can benefit from increasing the amount of memory to at least the size of the data set. Memory is also used to maintain the watchers, so if your use-case requires thousands of watchers, you can benefit from increasing the amount of RAM available to your deployment ever further.