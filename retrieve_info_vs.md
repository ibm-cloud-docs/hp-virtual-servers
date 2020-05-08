---

copyright:
  years: 2019
lastupdated: "2019-12-09"

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

# Retrieving information about a virtual server
{: #retrieve-info-vs}

After you created a virtual server by using the {{site.data.keyword.hpvs}} service, you can check the detailed information of your new instance.

1. Go to the [Resource list](https://cloud.ibm.com/resources){: external} and look for your virtual server instance under the **Services** entry in the **Name** column. You can open the **Resource list** either from the {{site.data.keyword.cloud_notm}} **Navigation Menu** or from the {{site.data.keyword.cloud_notm}} Dashboard by selecting **View resources** within the **Resource summary** area. The **Locate** map shows you in which region your instance was created, and column **Location** displays the data center where your instance was created. Use the filter to search for virtual server instances in certain regions or data centers. See also Figure 2 from [Provisioning a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-provision).
2. Click your virtual server instance to open the **{{site.data.keyword.hpvs}} dashboard**.

In this dashboard, you can find the public IP address, which you use to connect to the virtual server.
You must use the internal IP address when you connect to a virtual server from another virtual server, which is in the same virtual LAN (VLAN). VLANs span across all data centers within their region.

You must also work with the internal IP addresses, while connecting to other {{site.data.keyword.cloud_notm}} Cloud services, that are located within the same data center.

![**{{site.data.keyword.hpvs}}** dashboard](image/hpvs_instance.jpg "**{{site.data.keyword.hpvs}}** dashboard")
*Figure 1. **{{site.data.keyword.hpvs}}** dashboard*

In the **Connect** area, the dashboard offers a comfortable feature to log-in to the virtual server:
1. Open **How to connect**
2. Select your operating system from where to connect to your virtual server. The operating system you currently work on, is pre-selected.
3. Copy the shown command into the clipboard.
4. Paste it into a command line or command prompt of your operating system.

This method works, if you produced the private key by using the default settings (file name and location).
It also works, if you have configured your SSH client to automatically use the private key corresponding to the virtual server's public key.




The dashboard also displays the fingerprint of the SSH public key, with which the virtual server was created.
This fingerprint is displayed with the `ssh-keygen` command when you generate a pair of SSH keys. Or you can reproduce the fingerprint of a public SSH key by entering a command similar to the following one:

```
ssh-keygen -E sha256 -lf id_rsa.pub
```
{: codeblock}

- Parameter `-E` specifies the hash algorithm used to produce the fingerprint.
- Parameter `-lf` specifies the file (name and location, if necessary) of the public SSH key half, which you want to check.
