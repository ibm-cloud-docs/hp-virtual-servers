---

copyright:
  years: 2019
lastupdated: "2019-11-13"

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

#Provisioning a virtual server
{: #provision}

You can create a virtual server by using the {{site.data.keyword.hpvs}} service from the {{site.data.keyword.cloud_notm}}.
{:shortdesc}

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com){: external}.
2. Click **Catalog** from the menu bar and browse for the **Compute** category.
3. In the displayed selection of services, look for the **{{site.data.keyword.hpvs}}** tile. If you do not see the tile, enter `virtual server` in the search field. Then, click the tile. The {{site.data.keyword.hpvs}} page opens.
![Creating a virtual server](image/hpvs_create_instance.jpg "Creating a virtual server")
*Figure 1. Creating a virtual server*
4. In section **Select a region**, you can select the data center in which to create the virtual server instance. You can accept the preselected default data center.
5. The **Service name** field offers a name proposal for your virtual server. You can change this name according to your conventions.
6. In the **Tags** field, you can optionally add tags to organize your resources.
7. Enter your SSH public key into the **SSH public key** field. For more information about SSH keys, see [Generating SSH keys](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-generate_ssh).
8. Select the **Free Standard Plan - S** from the pricing plans. This plan is the preselected default. You can create only one virtual server with this plan.  
9. Click **Create** to provision a virtual server instance.

{:note}
As soon as you create a virtual server instance, a virtual LAN (VLAN) is transparently created or assigned. One VLAN is used within one region for one account. Each VLAN can contain up to five virtual servers per data center. A VLAN is deleted as soon as you delete the last virtual server instance that is assigned to this VLAN.

The virtual server instance appears in the **Resource list**. Using your browser's refresh function, you can check whether the instance's status did switch from **Provision in progress** to **Provisioned**. Due to configuration processes, this can take up to 30 minutes.



![Resource list with a virtual server](image/hpvs_resource_list.jpg "Resource list with a virtual server")
*Figure 2. Resource list with a virtual server*
