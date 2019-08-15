---

copyright:
  years: 2019
lastupdated: "2019-08-16"

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

You can read the following FAQs to help you with {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}}.

## Are there any backup functions provided for my virtual server?

* IBM cannot provide any backup functions for your server instance. This is due to the access restrictions of IBM system administrators and to software configuration
on your virtual server. If you require backup capabilities, you need to configure your virtual server backup in your own responsibility.

## What is the purpose of the private IP address?

* The private IP address must be used when connecting to a virtual server from another virtual server or connecting from another cloud service that is running in the same data center.  

## How do I provide my applications in high availability?
* Do not rely on just one virtual server instance. Instead, your goal should be to run your application on multiple instances in combination with a load balancer to ensure high availability.
Ideally, these instances are spread across multiple regions and availability zones. With {{site.data.keyword.hpvs}}, virtual servers are provided within one region (us-south) and one availability zone (dal10-01). <!-- We are
working on providing additional regions and zones. The currently available locations will be
selectable in the provisioning UI as soon as there are multiple available.  -->

## Will my virtual server instance be available 24 / 7?
* The virtual server is running on infrastructure that needs to be maintained. Before maintenance, the virtual servers must be stopped. Customers are notified about
maintenance schedules in advance. Service availability is defined within the "Terms of Conditions" of the {{site.data.keyword.hpvs}} offering.

## Can I create more than one virtual server instance within my {{site.data.keyword.cloud_notm}} account?
* Currently you can create only one virtual server instance in your {{site.data.keyword.cloud_notm}} account.
