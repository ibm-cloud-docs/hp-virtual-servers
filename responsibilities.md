---

copyright:
  years: 2019, 2021
lastupdated: "2021-04-15"

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

# Responsibilities when you use {{site.data.keyword.hpvs}}
{: #responsibilities_hpvs}

Learn about management responsibilities and terms and conditions that you must observe when you use {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}}.
{: shortdesc}

## Management responsibilities
{: #responsibilities_mgmt_hpvs}

IBM provides you with an enterprise cloud platform to deploy your workload. You choose how to set up, integrate, back up, and operate your workloads in the cloud.

### Cloud Infrastructure
{: #responsibilities_ci_hpvs}

IBM's responsibilities:
- Deploy a fully managed, reliable, secured, IBM-owned infrastructure.
- Fulfill requests for {{site.data.keyword.hpvs}} across multiple data centers.

Your responsibilities:
- Take the required actions on regular maintenance notification from IBM.
- Use the provided UI console to provision compute and storage to meet the needs of your workload.
- Design and deploy your workload in a way that achieves high availability by using multiple data centers.

### {{site.data.keyword.hpvs}}
{: #responsibilities_hpvs_hpvs}

IBM's responsibilities:
- Set up virtual servers as requested.
- Update the OS kernel.

Your responsibilities:
- Maintain OS patches, version updates, and security updates.
- Set up your backup and recovery strategies for workload data.
- Monitor the health of your workload.

### Security-rich Environment
{: #responsibilities_secenv_hpvs}

IBM's responsibilities:
- Maintain controls commensurate to current industry compliance standards.
- Enable security features, such as encrypted disks.
- Continuously monitor stock images to detect vulnerability and security compliance issues.
- Use the {{site.data.keyword.cloud}} user interface and CLI for authentication and authorization for the  {{site.data.keyword.hpvs}} offering.

Your responsibilities:
- Use security groups and network ACLs to secure your virtual server instances, such as restricting what IP addresses can SSH into the instance.
- Choose how to connect your workload to the public internet.
- Restrict user access to the appropriate resources and resource groups.

### App Orchestration
{: #responsibilities_app_hpvs}

IBM's responsibilities:
- None.

Your responsibilities:
- Use the tools and features that are provided to:  
   - Configure and deploy setup permissions.
   - Integrate with other services.
   - Externally serve, monitor the health, save, back up, and restore data.
   - Manage your highly available and resilient workloads.
- Design and deploy your workload in a way that achieves high availability by using multiple data centers.

### Connecting clients
{: #responsibilities_connect_hpvs}

IBM's responsibilities:
- None.

Your responsibilities:
- Make sure that the clients, which can connect to the virtual server instances are maintained and securely configured. Also, don't share private keys to authenticate to any virtual server.



## Abuse of {{site.data.keyword.cloud}} Virtual Private Cloud
{: #terms}

Clients must not misuse {{site.data.keyword.cloud}} Infrastructure.

Misuse includes:
- Any illegal activity
- Distribution or execution of malware
- Harming IBM Cloud Infrastructure or interfering with the use of IBM Cloud Infrastructure
- Harming or interfering with the use of any other service or system
- Unauthorized access to any service or system
- Unauthorized modification of any service or system
- Violation of the personal rights of others

See [Cloud Services terms](/docs/overview/terms-of-use?topic=overview-terms) for overall terms of use.
