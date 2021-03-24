---

copyright:
  years: 2019, 2020
lastupdated: "2021-03-23"

subcollection: hp-virtual-servers

keywords: high availability, disaster, recovery

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# High availability and disaster recovery
{: #disaster_hpvs}

The {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} service runs on a highly available and reliable IBM LinuxONE infrastructure with no single point of failure. However, a single virtual server can still have an outage in a disaster scenario. Therefore, deploy your workload in active-active mode across multiple virtual server instances, which are running in different data centers (for example, `Dallas 10`,`Dallas 12`, and `Dallas 13`). This setup ensures an operable workload with fault tolerance on the underlying virtual servers.

Example workloads that you can deploy this way are databases (PostgreSQL, MongoDB, or MySQL) or applications with no local state.
{:shortdesc}

If the latency requirements or types of workload do not allow to run an active-active configuration across data centers, you can perform regular backups from one virtual server to another instance in a different data center. In a disaster, the amount of lost data depends on the frequency of the backups and the time to restore a backup.  

## Backup
{: #disaster_backup}

Back up your data from business-relevant virtual servers (primary virtual servers) to recovery virtual server instances. A virtual server that is created with {{site.data.keyword.hpvs}} is configured with two disks. One for the operating system and the other one for your data (mounted as `/data/`). One possible way to back up the data disk is to set up a `cron job`, which copies content of the disk to a recovery virtual server instance. Choose the appropriate backup frequency for your workload. For example, if you accept a maximum loss of data changes of 1 hour, add the following text file (for example, cron_backup) to `/etc/cron.hourly`:

```
#!/bin/sh

rsync /data <public HPVS IP or internal HPVS IP>:/data
```
{: codeblock}

On Ubuntu, you install the file-copying tool `rsync` on the primary virtual server in two steps:

Initially, on the primary virtual server, use the following command to synchronize the package index files from their sources. This is required to retrieve information about available packages, versions, and dependencies.

```
apt-get update
```
{: codeblock}

Then install the file-copying tool `rsync` on the primary virtual server:

```
apt-get install rsync
```
{: codeblock}

You must exchange the SSH keys so that the cron job script can run. How this works depends on the backup server's SSH server configuration. With the default configuration, you must place the public SSH key as a line in the `$HOME/.ssh/authorized_keys` file of the user that is used on the backup server. To make `rsync` use the private SSH key, you must place this key in the cron-jobs user's `$HOME/.ssh` and give it a default name, for example, for rsa keys, use the name  `id_rsa`. Other options are to configure the SSH client for this user or to adjust the `rsync` command accordingly.

If needed, quiesce the concerned application before the backup operation. If the primary and the recovery virtual servers are located in one region, use the internal IP address for the backup.  

If you want to maintain multiple backups outside virtual server instances created with {{site.data.keyword.hpvs}}, you can package the data with `tar`, encrypt it with `GnuPG` and store it in {{site.data.keyword.cloud}} Object Storage.

Also, install the application to the recovery virtual server instance to enable a quick failover.

Finally, always access the application that should be recoverable by using a URL that is pointing to the virtual server IP address and never access the IP address directly.

In a recovery scenario, you can then adjust the URL to point to the recovery virtual server).

## Recovery
{: #disaster_recovery}

To recover from a disaster by using the previously described backup environment, perform the following steps:

1.  Connect to the recovery virtual server instance.
2.  Start the concerned application.
3.  Test whether the application starts successfully.
4.  Reconfigure the DNS to map the application URL to the recovery virtual server.
5.  Test whether the application is reachable from outside as expected.

 Test the recovery procedure periodically to ensure its effectiveness.
