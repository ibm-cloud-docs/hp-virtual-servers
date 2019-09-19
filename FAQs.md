---

copyright:
  years: 2019
lastupdated: "2019-09-19"

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# FAQs
{: #faqs}

You can read the following FAQs to help you with {{site.data.keyword.hpvs}}.

## Are there any backup functions provided for a virtual server?

* {{site.data.keyword.hpvs}} does not provide any backup functions for your server instance. If you require backup capabilities, you need to configure your virtual server backup in your own responsibility.

## What is the purpose of the internal IP address?

* You must use the internal IP address when you connect to a virtual server from another virtual server. You must also use it when you connect from another cloud service that is running in the same data center.  

## How do I provide my applications in high availability?
* Do not rely on just one virtual server instance. Instead, run your application on multiple instances in combination with a load balancer to ensure high availability.
Ideally, these instances are spread across multiple regions and availability zones. With {{site.data.keyword.hpvs}}, virtual servers are provided within one region (us-south) and one availability zone (dal10-01). <!-- We are
working on providing additional regions and zones. The currently available locations will be
selectable in the provisioning UI as soon as there are multiple available.  -->

## Is a virtual server instance available 24 / 7?
* The virtual server is running on infrastructure that needs to be maintained. Before maintenance, the virtual servers must be stopped. Customers are notified about
maintenance schedules in advance. Service availability is defined within the [{{site.data.keyword.cloud}} Service Description](https://www-03.ibm.com/software/sla/sladb.nsf/pdf/6605-17/$file/i126-6605-17_06-2019_en_US.pdf){: external}. <!-- of the {{site.data.keyword.hpvs}} offering. -->

## Can I create more than one virtual server instance within my {{site.data.keyword.cloud_notm}} account?
* In the Free Standard pricing plan, you can create exactly one virtual server instance in your {{site.data.keyword.cloud_notm}} account.

## Can I provide a custom image, derive a custom image from a virtual server instance (HP-VS instance), or clone an HP-VS instance?
* Currently, you are not able to provide a custom image. Also, you cannot create a custom image from an existing HP-VS instance, nor clone an instance.
These limitations are due to the fact that IBM's system administrators are very restricted in accessing your HP-VS instance.
