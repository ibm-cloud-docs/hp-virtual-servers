---

copyright:
  years: 2019, 2023
lastupdated: "2022-07-20"

subcollection: hp-virtual-servers

keywords: troubleshooting, support, help, virtual servers help

---

{{site.data.keyword.attribute-definition-list}}

# FAQs
{: #faqs}

You can read the following FAQs to help you with {{site.data.keyword.hpvs}}.

## Are there any backup functions provided for a virtual server?
{: #faq1}
{: faq}
{: support}

{{site.data.keyword.hpvs}} does not provide any backup functions for your virtual server instances. If you require backup capabilities, you need to configure your virtual server backup in your own responsibility.

## What is the purpose of the internal IP address?
{: #faq2}
{: faq}
{: support}

You must use the internal IP address when you connect to a virtual server from another virtual server, which is in the same virtual LAN (VLAN).

## How do I provide my applications in high availability?
{: #faq3}
{: faq}
{: support}

Do not rely on just one virtual server instance. Instead, run your application on multiple instances in combination with a load balancer to ensure high availability.

Ideally, these virtual servers are spread across multiple regions and data centers. With {{site.data.keyword.hpvs}}, virtual servers are provided within two regions (`us-south` and `eu-de`) and nine data centers (`Dallas 10`, `Dallas 12`, `Dallas 13`, `Frankfurt 02`, `Frankfurt 04`, `Frankfurt 05`, `Washington 04`, `Washington 06`, and `Washington 07`).

## Can I have more than five virtual servers within one data center?
{: #faq4}
{: faq}
{: support}

Currently, {{site.data.keyword.hpvs}} supports only five virtual server instances per account in each data center. To provision more instances, you can use multiple accounts or data centers, or both.

## Is a virtual server available 24 / 7?
{: #faq5}
{: faq}
{: support}

The virtual server is running on infrastructure that needs to be maintained. Before maintenance, stop the virtual servers. You are notified about
maintenance schedules in advance. Service availability is defined within the [{{site.data.keyword.cloud}} Service Description](https://www-03.ibm.com/software/sla/sladb.nsf/pdf/6605-17/$file/i126-6605-17_06-2019_en_US.pdf){: external}.

## Does the data center have an impact on performance and reliability of a virtual server?
{: #faq6}
{: faq}
{: support}

No, all of the data centers are of the same quality. For best availability, follow the hints in question [How do I provide my applications in high availability?](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-faqs#faq3).

## How can I figure out in which data center a virtual server is located?    
{: #faq7}
{: faq}
{: support}

This information is visible on both the **{{site.data.keyword.hpvs}} dashboard** and the **Resource list (**see [Provisioning a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-provision) and [Retrieving information about a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs)).

## Can I rearrange a server to another data center?    
{: #faq7a}

This action is not possible.  

## Can I provide a custom image, derive a custom image from a virtual server, or clone a virtual server?
{: #faq8}
{: faq}

Currently, you are not able to provide a custom image. Also, you cannot create a custom image from an existing virtual server instance, nor clone an instance.
The reason for these limitations is that IBM system administrators are restricted in accessing your virtual servers.

## Can IBM system administrators provide me access to my virtual server instance, if I lose my private key?
{: #faq9}
{: faq}
{: support}

IBM system administrators cannot recover access to your virtual server, as they do not have the required privileges. Therefore, establish a business continuity and disaster recovery plan (BCDR plan). With a BCDR plan available, you can delete the lost virtual server, create a new one, and restore the data of the old instance to the new one.

## How can I install software?
{: #faq10}
{: faq}
{: support}

The virtual servers that are created with the {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} service are currently provided with an Ubuntu Linux operating system. Investigate how to install software and applications in Ubuntu by using the command line and select the way that is most appropriate for your use case.

## How can I adjust the firewall of my virtual server?
{: #faq11}
{: faq}

A newly generated {{site.data.keyword.cloud}} virtual server, which runs with an Ubuntu Linux operating system, has **question**, the Linux firewall utility, preinstalled. Investigate, what firewall tool you want or need to use in your environment.

IBM provided adjustments for **IPtables** on a virtual server.
You can apply your own adjustments on a default configuration for **IPtables** as described in
[Protecting a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-protect_vs).

## How do I authorize more SSH public keys for my virtual server?
{: #faq12}
{: faq}
{: support}

With the OpenSSH Server component that is running in its standard configuration on your virtual server, you can manage authorized keys in a file. This file is located by default in a user's home directory:
`<user_home>/.ssh/authorized_keys`. Add an SSH public key in a new line into this file. Then, the user can connect to the virtual server with the pertaining SSH private key. For more information, read the OpenSSH documentation.

## Can I load or unload a kernel module?
{: #faq13}
{: faq}

These actions are not supported.

## What IBM catalog offerings work with {{site.data.keyword.hpvs}}?
{: #faq14}
{: faq}
{: support}

In general, all offerings work with Hyper Protect Virtual Servers except for offerings that work explicitly only with classic infrastructure or VPC infrastructure. These offerings do not work with Hyper Protect Virtual Servers.

## I cannot access my server after a restart because the boot disk is almost full. How can I recover the server?
{: #faq15}
{: faq}

You can't recover your server because the boot disk is not resizable. You need to create a new server and restore your backups to them. Consider monitoring your server resource usage to prevent the same problem in the future. Verify that your file system usage is according to the recommendations in [{{site.data.keyword.hpvs}} file system characteristics](/docs/hp-virtual-servers?topic=hp-virtual-servers-protect_vs#hpvs_fs).
