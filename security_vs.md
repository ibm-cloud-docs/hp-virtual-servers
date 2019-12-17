---

copyright:
  years: 2019
lastupdated: "2019-12-12"

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

{{site.data.keyword.hpvs}} is a service that is used to provide highly secure virtual servers. The difference to common virtual servers from a security perspective is that an instance that is created from this {{site.data.keyword.cloud}} service is running on a secured stack. Even {{site.data.keyword.cloud_notm}} system administrators cannot access your data nor track your usage. They also do not have insight into the security status of your virtual server. In addition, they are limited in setting up or changing the configuration of your virtual server.
{:shortdesc}


However, the provided virtual server is still a virtual server – which is accessible from the internet. From a security perspective, you need to protect the virtual server instance itself.


For Ubuntu, different **System Hardening Guide** documents are available.

The Ubuntu operating system that is provided for your virtual servers is partwisely hardened according to the CIS Ubuntu Linux 18.04 LTS Benchmark, Level 1- Server profile.
Therefore, it is more secure than the standard Ubuntu image. However, it still may not be secure enough according to your requirements.
Select a **System Hardening Guide** depending on your companies’ location and policies.

The subsequent description presents an overview of the features implemented for the Ubuntu OS offered with {{site.data.keyword.hpvs}}.   

As part of the hardening, IBM installed and configured AIDE (Advanced Intrusion Detection
Environment), which is a file and directory integrity checker. With the implemented configuration, AIDE is
run as a `cron job` every 5 minutes while the server is running.

The firewall accepts all outgoing TCP, UDP and ICMP traffic.
Incoming traffic is only accepted on ports 22 (TCP), 80 (TCP), 443 (TCP), 8080(TCP), 53(TCP and UDP), and with protocol ICMP.

For firewall configurations based on **iptables**, IBM provided an
`init.d`-service which is located within `/etc/init.d/firewall`. In order to adjust the firewall,
configure your **iptables** and override the existing configuration file by using the following command:

```
iptables-save > /etc/iptables/iptables.conf
```
{: codeblock}

Configure the firewall as restrictive as
possible in order to secure your virtual server and data. For example, you can limit the access for which ports can be used with which
network interface. It is in your responsibility to configure the firewall correctly and to protect
your network traffic. For example, ensure that your network traffic is encrypted.

The following commands open the firewall for incoming traffic on a specified protocol and port:

`iptables -A INPUT -p <protocol> --dport <port> -m`
`state --state NEW -j ACCEPT`

In order to make the configuration persistent, save your changes as described.
Otherwise, all configuration updates are lost after a reboot.
