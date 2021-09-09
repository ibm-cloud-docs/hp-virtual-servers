---

copyright:
  years: 2019, 2020
lastupdated: "2021-08-19"

subcollection: hp-virtual-servers

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Protecting a virtual server
{: #protect_vs}

{{site.data.keyword.hpvs}} is a service that is used to provide highly secure virtual servers. The difference to common virtual servers from a security perspective is that an instance that is created from this {{site.data.keyword.cloud}} service runs on a secured stack. Even {{site.data.keyword.cloud_notm}} system administrators can't access your data nor track your usage. They don't have an insight into the security status of your virtual server. They are also limited in how they can set up or change for your virtual server configuration.
{: shortdesc}


However, the virtual server is still a virtual server, which is accessible from the internet. From a security perspective, you need to protect the virtual server instance itself.


Do not use personal information, for example, your name, as the instance name or as part of the instance name. The data that you provide when you provision an instance or interact with the hpvs cli is not considered to be personal data or credentials.
Learn more about IBM Cloud Hyper Protect Virtual Servers' Data usage and Certifications [here](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=165C6EF0FFDA11E8BABD512A6952EE1F)
{: note}

For Ubuntu, different **System Hardening Guide** documents are available.

The Ubuntu operating system that is provided for your virtual servers is part wisely hardened according to the CIS Ubuntu Linux 20.04 LTS Benchmark, Level 1- Server profile, which means that the operating system is more secure than the standard Ubuntu image. However, it still might not be secure enough to meet to your requirements.
Select a **System Hardening Guide** depending on your companies’ location and policies.

The subsequent description presents an overview of the features that are implemented for the Ubuntu OS offered with {{site.data.keyword.hpvs}}.   

As part of the hardening, IBM installed and configured AIDE (Advanced Intrusion Detection
Environment), which is a file and directory integrity checker. With the implemented configuration, AIDE is
run as a `cron job` every 5 minutes while the server is running.

The firewall allows all outgoing TCP, UDP, and ICMP traffic. Incoming traffic is only accepted on ports 22 (the TCP port) and with protocol ICMP.

For firewall configurations based on **IPtables**, IBM provided an `systemd-service`, which is located within `/usr/lib/systemd/system/iptables.service` and added to the multi-user target.

To adjust the firewall, configure your **IPtables** and override the existing configuration file by using the following command:

```
iptables-save > /etc/iptables/iptables.conf
```
{: codeblock}

It is in your responsibility to configure the firewall correctly and to protect
your network traffic. Configure the firewall as restrictively as
possible to secure your virtual server and data. For example, ensure that your network traffic is encrypted and limit port access for the network interface.

The following commands open the firewall for incoming traffic on a specified protocol and port:

`iptables -A INPUT -p <protocol> --dport <port> -m`
`state --state NEW -j ACCEPT`

To make the configuration persistent, save your changes as described.
Otherwise, all configuration updates are lost after a restart.

Another part of the hardening is that the server is configured in such a way that the password expires after 90 days. After your password expires, you have 30 days to change it. If you don't change your password within the 30 days, your account becomes inactive and it is no longer possible to log in with SSH even if you are using SSH-keys. This restriction also includes system-accounts, for example, accounts created by using the command, `useradd --system`.

## Monitoring a virtual server
{: #monitor_a_vs}

It is best practice to:
- Monitor your virtual server instance resource usage, such as CPU, memory, and storage.
- Configure alerts that warn you if a resource is exhausted or almost exhausted.

Monitoring your server ensures that it has sufficient resources available to keep it running. For example, if you have no storage alerts configured and your boot disk (25 GB) is full, you can't restart your server. You can't recover your server because your boot disk can't be resized.

## File system characteristics
{: #hpvs_fs}

The {{site.data.keyword.hpvs}} file system layout consists of one file system for the root and one file system for the /data mount points.

If the file system is full, your instance can break, which means you can no longer log on to your instance.
The {{site.data.keyword.hpvs}} do not come with swap space configured, and it is not supported to add swap to the system.

Within `/tmp` is a `tmpfs`, which can use a large volume of RAM. Use `/var/tmp` for your applications when you need large volumes of temporary storage. Insufficient RAM can cause your system to behave unpredictably.

### Configuring logging for the Ubuntu server
You must manually configure `logrotate` and `journald` to leave a small percentage of the file systems free. To do this, configure `SystemKeepFree=15%` in `/etc/systemd/journald.conf`.

Check regularly that a percentage of the file system is still free. For the `logrotatation` configuration, make sure that your log files do not get too large. Compress and remove log files (or move them to your data volume) before they break your system.

Logging configuration recommendations:
- Create a non-root user for each application and run each application with that non-root user.
- Configure the application's logging and log rotation as the user who runs the application.
