---

copyright:
  years: 2023
lastupdated: "2023-10-26"

subcollection: hp-virtual-servers

keywords: virtual server instance, instance, migrating, migration, virtual server

---

{{site.data.keyword.attribute-definition-list}}


# Why migrate?
{: #why-migrate}

{{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) is the strategic direction from {{site.data.keyword.cloud_notm}}. New services and capabilities are available in the VPC environment.

{{site.data.keyword.cloud_notm}} VPC is a highly resilient, scalable and secure software-defined network (SDN) on which one can build isolated private clouds for business operations while maintaining essential public cloud benefits. You can choose preferred compute, storage and networking resources from a variety of cost-effective options for different workload demands. It also provides more capabilities and options regarding storage attachment to a virtual server. It is ideal for deploying cloud native applications.

{{site.data.keyword.cloud_notm}} Virtual Servers for Classic Infrastructure operate on native subnet and VLAN networking to communicate with each other within a data center (and single pod). {{site.data.keyword.cloud_notm}} Virtual Servers for VPC operate with an additional network orchestration layer that eliminates the pod boundary, creating increased capacity for scaling instances. 

In addition, the LinuxONE systems in the Classic Infrastructure are only collocated to the remaining IBM Cloud infrastructure, which limits the network interconnection and integration, and has implications on the underlaying storage infrastructure not being common with the IBM Cloud Classic Infrastructure Storage subsystem. You can learn more about the differences with VPC [here](/docs/cloud-infrastructure?topic=cloud-infrastructure-compare-infrastructure).

{{site.data.keyword.hpvs}} (HPVS) for Classic is based on Secure Service Container technology where the secure enclave (trusted execution environment) is the LPAR.

{{site.data.keyword.hpvs}} for VPC is based on [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=virtualization-introducing-secure-execution-linux){: external} technology introduced with IBM z15 / LinuxONE III. The secure enclave becomes more granular and is at the Virtual Machine level. The following table shows few key differences: 


| IBM Cloud Feature | {{site.data.keyword.hpvs}} for Classic | {{site.data.keyword.hpvs}} for VPC |
|--------------|--------------|--------------|
| Protection boundary | LPAR |  Virtual Machine   | 
| Geographic availability | DAL, WDC, FRA | LON, TOR, TOK, SAO, WDC, MAD, and more to come | 
| Logging/Monitorint | Integrated Logging and Monitoring     |  LogDNA and rsyslog  | 
| Instance profiles | 4 T-shirt sizes   | 13 profiles     | 
| Pricing/Billing granuality | Monthly (starting $170/month)  | Hourly (starting $110/month)    | 
| Workload integrity  | Docker Content Trust   |  Docker Content Trust, Digest, and RedHat Simple Signing   |  
{: caption="Table 1. Key differences between {{site.data.keyword.hpvs}} for Classic and for VPC" caption-side="bottom"}


All new and key differentiated capabilities will be built only for {{site.data.keyword.hpvs}} for VPC.  The following list are a few examples and [a blog](https://www.ibm.com/blog/announcement/enhanced-security-and-scalability-enabled-for-hyper-protect-virtual-servers-hpvs-for-virtual-private-cloud-vpc/){: external} published recently: 

1.	Encrypted catalog image with a hardware based root of trust
2.	Encrypted Multi-Party contract to apply zero-trust concepts and separation of duty
3.	Attestation record of deployment with a 3rd party certificate authority root of trust
4.	Multi-OCI and multi-volume support for more complex containized workloads and use cases
5.	Bring-Your-Own-Key with Hyper Protect Crypto Service to protect persistent data volumes
6.	Private Container Registries and Public Key Infrastructure (PKI) support

IBM strongly recommends {{site.data.keyword.hpvs}} for VPC to leverage advantages of VPC and all the latest {{site.data.keyword.hpvs}} capabilities rolled out regularly for customers.

Customers will not incur any cost for migration. IBM will provide all the needed guidance and support to facilitate a smooth migration journey. Any cost incurred as part of the migration process will be waived via a promo code that can be requested by contacting your local sales representative or sending an email to [zaas.client.acceleration@ibm.com](mailto:zaas.client.acceleration@ibm.com).
{: note}


## Get started today
{: #get-started}

For {{site.data.keyword.hpvs}} for VPC, the provisioning, deployment, and management all occur through the [{{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}}](https://cloud.ibm.com/vpc-ext/provision/vs?architecture=s390x&secureExecution=true) page. For more details, refer to the [product page](https://www.ibm.com/products/hyper-protect-virtual-servers) or [documentation](/docs/vpc?topic=vpc-about-se).