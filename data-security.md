---

copyright:
  years: 2020, 2023
lastupdated: "2020-10-13"

subcollection: hp-virtual-servers

keywords: data encryption in {{site.data.keyword.hpvs}}, data storage for {{site.data.keyword.hpvs}}, bring your own keys for {{site.data.keyword.hpvs}}, BYOK for{{site.data.keyword.hpvs}}, key management for {{site.data.keyword.hpvs}}, key encryption for {{site.data.keyword.hpvs}}, personal data in {{site.data.keyword.hpvs}}, data deletion for {{site.data.keyword.hpvs}}, data in {{site.data.keyword.hpvs}}, data security in {{site.data.keyword.hpvs}}

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data in {{site.data.keyword.hpvs}}
{: #mng-data}

{{site.data.keyword.hpvs}} is deprecated. As of 18 August 2024, you can’t create new instances, and access to free instances will be removed. Existing premium plan instances are supported until 31 January 2025. Any instances that still exist on that date will be deleted.
{: deprecated}

To ensure that you can securely manage your data when you use {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}}, it is important that you know exactly what data is stored and encrypted and how you can delete any personal data. All data that is stored on {{site.data.keyword.hpvs}} disks is automatically encrypted and the encryption key is stored on a secure enclave.
{: shortdesc}


## How your data is stored and encrypted in {{site.data.keyword.hpvs}}
{: #data-storage}

{{site.data.keyword.hpvs}} uses the following methods to protect your data:

- {{site.data.keyword.hpvs}} is built on [IBM Secure Service Container technology](https://www.ibm.com/us-en/marketplace/secure-service-container){: external}, and provides workload isolation, restricted administrator access and tamper protection for the underlying host system.
- The default SSH connection uses your private and public keypair for authentication and the connection is encrypted automatically.
- [Identity and Access Management (IAM)](/docs/hp-virtual-servers?topic=hp-virtual-servers-iam-hpvs) secures access to the {{site.data.keyword.cloud_notm}} account, the {{site.data.keyword.hpvs}} management Web user interface and CLI.
- All disks attached to a {{site.data.keyword.hpvs}} are provided on storage encrypted with LUKS according to AES-256. The default keys are managed by the underlying [IBM Secure Service Container technology](https://www.ibm.com/us-en/marketplace/secure-service-container){: external}.

## Protecting your sensitive data in {{site.data.keyword.hpvs}}
{: #data-encryption}

### Data at rest
{: #data-atrest}

The data that you store in {{site.data.keyword.hpvs}} is encrypted securely at rest by using a randomly generated key, which the underlying [IBM Secure Service Container technology](https://www.ibm.com/us-en/marketplace/secure-service-container){: external} manages. Use the Linux command-line tools to delete data in a virtual server.

### Data in flight
{: #data-inflight}


The IBM LinuxONE infrastructure components for the {{site.data.keyword.hpvs}} service are situated in colocation with the data centers. This means that these components are placed in the same data centers as the {{site.data.keyword.cloud_notm}} infrastructure but have their own network setup, which affects the network connection.

![{{site.data.keyword.hpvs}}** architecture network isolation](image/hpvs_architecture-vs_network_overview.svg "{{site.data.keyword.hpvs}} architecture network isolation"){: caption="Figure 1. {{site.data.keyword.hpvs}} Architecture network isolation" caption-side="bottom"}


It is your responsibility to protect the connection for any application that runs in your {{site.data.keyword.cloud_notm}} and it is recommended that you follow these guidelines:
* You can either use the public route or a private route through [{{site.data.keyword.cloud_notm}} Service Endpoints](https://cloud.ibm.com/docs/account?topic=account-service-endpoints-overview){: external} if you want to connect any other service to a {{site.data.keyword.hpvs}} instance. You can find a list of all {{site.data.keyword.cloud_notm}} services, which support {{site.data.keyword.cloud_notm}} Service Endpoints [here](https://cloud.ibm.com/docs/account?topic=account-vrf-service-endpoint#use-service-endpoint){: external}. It is recommended that you use a protected connection if you use the public route, for example, a VPN.
* Public IP addresses are mapped to internal IP addresses on all ports. Make sure you open only the ports on your virtual server that you want to be exposed to the public network.
* Use the internal IP address to connect from one virtual server to another in the same region via the internal network. Your traffic on the internal network is automatically isolated from other tenants.

### Deleting {{site.data.keyword.hpvs}} instances
{: #service-delete}

Use the [web user interface](/docs/hp-virtual-servers?topic=hp-virtual-servers-remove_vs) or the [CLI](https://cloud.ibm.com/docs/hpvs-cli-plugin#hpvs-instance-delete) to delete your instance. The virtual server and all the data that is stored in it is deleted after the 7-day reclamation period, as described [here](/docs/hp-virtual-servers?topic=hp-virtual-servers-remove_vs).

The {{site.data.keyword.hpvs}} data retention policy describes how long your data is stored after you delete the service. The data retention policy is included in the IBM Cloud Hyper Protect Virtual Servers service description, which you can find in the [Terms and Notices](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-8680-01).
