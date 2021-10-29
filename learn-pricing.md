---
copyright:
  years: 2019, 2020
lastupdated: "2020-02-17"

keywords: etcd, databases, pricing, resources, scaling

subcollection: databases-for-etcd

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Pricing
{: #pricing}

An {{site.data.keyword.databases-for-etcd_full}} Standard plan deploys as one highly available etcd cluster with three data members. Your data is replicated across members. The Standard plan is priced based on the total amount of disk storage, RAM, dedicated cores, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.databases-for-etcd}} deployments have a minimum of 20 GB of disk (200 IOPS) and 1 GB of RAM per data member. Review the [documentation on performance](/docs/databases-for-etcd?topic=databases-for-etcd-performance) to see the importance of accounting for IOPS in your capacity planning. 

## Cost Breakdown

**Disk storage per data member** - gigabytes of disk that are allocated to a Databases for etcd data member, or the size of your data.  
**RAM per data member** - gigabytes of RAM that are allocated to a Databases for etcd data member.  
**Backup storage** - amount of storage used for backups by a Databases for etcd deployment.

Resources | Breakdown | Price
-------|-------|-------
20 GB-Month disk | 3 members x 20 GB x $0.58 | $34.80
1 GB-Month RAM | 3 members x 1 GB  x $5 | $15
{: caption="Table 1. Pricing example for two data members" caption-side="top"}

Total per month = $49.80/Month  
Total per hour = $.068/Hour

All prices here are in US dollars. To see pricing in your local currency, you can to use the pricing calculator.
{: .tip}

## IBM Cloud Databases enabled by IBM Cloud Satellite Pricing

{{site.data.keyword.databases-for-etcd}} deployments are deployable into IBM Cloud Satellite locations. The management fee for these Cloud Databases is $45 per vCPU per month, with a 6 vCPU minimum.

Resources | Breakdown | Price
-------|-------|-------
6 vCPUs per month | 2 members x 6 GB x $45 | $540
{: caption="Table 2. Pricing example for  6 vCPUs and two data members" caption-side="top"}

Total per month = $49.80/Month

## Using the Pricing Calculator

Templates are provided for ease of use and provide balanced resource allocations appropriate for general purpose workloads. The **Custom** tab can be used to configure Disk, RAM, and vCPU, as desired.

For pricing estimation, use the **Add to Estimate** button at the bottom of the [{{site.data.keyword.databases-for-etcd}} catalog page](https://cloud.ibm.com/catalog/databases-for-etcd). Input your total consumption across two data members into the calculator. For example, 20 GB of disk and 1 GB of RAM across three data members would be priced at 60 GB of disk and 3 GB of RAM respectively.

![Pricing calculator estimation with 20 GB of disk and 1 GB of RAM, per member](images/pricing-calc.png)

## Backups Pricing

Users also receive their total disk space purchased, per database, in free backup storage. For example, in a month, if you have a {{site.data.keyword.databases-for-etcd}} deployment that has provisioned 20 GB of disk per member, which has three data members, you receive 60 GB of backup storage free for that month. If your backup storage utilization is greater than 60 GB for the month in this scenario, each gigabyte is charged at an overage $0.03/month. Most deployments will not ever go over the allotted credit.

## Dedicated Cores Pricing

You have the option of selecting the CPU allocation for your deployment. With dedicated cores, your resource group is given a single-tenant host with a guaranteed minimum reserve of cpu shares. Your deployments are then allocated the number of CPUs you specify. The cost of dedicated cores is $30 per core per month, and each member gets the selected number of cores. For example, if you provision a deployment with 3 dedicated cores per member, that is a total of 9 cores, and billed at $270 per month. 

Dedicated cores are an optional feature. The default `Shared CPU` setting provisions your deployment on hosts with shared compute resources and incurs no additional charge.

## Scaling per Member

{{site.data.keyword.databases-for-etcd}} deployments have minimum and maximum allocation for disk and RAM as shown. Scaling deployments through the API/CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member.

Resource | Minimum | Maximum | Scaling Granularity (API/CLI)
----------|-----|-----|-------
Disk | 5 GB per member | 4 TB per member | 1024 MB per member
RAM | 1 GB per member | 112 GB per member | 128 MB per member
CPU (if enabled) | 3 CPUs per member | 28 CPUs per member| 1 CPU per member
{: caption="Table 2. Per Member Scaling Limits" caption-side="top"}


