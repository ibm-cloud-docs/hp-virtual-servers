---

copyright:
  years: 2019
lastupdated: "2019-11-13"

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

1. Go to the [Resource list](https://cloud.ibm.com/resources){: external} and look for your virtual server instance under the **Services** entry in the **Name** column. You can open the **Resource list** either from the {{site.data.keyword.cloud_notm}} **Navigation Menu** or from the {{site.data.keyword.cloud_notm}} Dashboard by selecting **View resources** within the **Resource summary** area. Column **Location** displays the selected data center where your instance was created. Use the filter to search for virtual server instances in certain regions or data centers. See also Figure 2 from [Provisioning a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-provision).
2. Click your virtual server instance to open the **{{site.data.keyword.hpvs}} dashboard**.

In this dashboard, you can find the public IP address, which you use to connect to the virtual server.
You must use the internal IP address when you connect to a virtual server from another virtual server, which is in the same virtual LAN (VLAN).
VLANs span across all data centers within their region.

![**{{site.data.keyword.hpvs}}** dashboard](image/hpvs_instance.jpg "**{{site.data.keyword.hpvs}}** dashboard")
*Figure 1. **{{site.data.keyword.hpvs}}** dashboard*

The dashboard also displays the fingerprint of the SSH public key, with which the virtual server was created.
This fingerprint is displayed with the `ssh-keygen` command when you generate a pair of SSH keys. Or you can reproduce the fingerprint of a public SSH key half by entering a command similar to the following one:

```
ssh-keygen -E sha256 -lf id_rsa.pub
```
{: codeblock}

- Parameter `-E` specifies the hash algorithm used to produce the fingerprint.
- Parameter `-lf` specifies the file (name and location, if necessary) of the public SSH key half, which you want to check.
