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

# Managing user access
{: #manage_access}

The {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} service provides you with virtual servers on a highly
secure stack with an Ubuntu Linux operating system.
{:shortdesc}

A good practice is to have different operating-system-based users for each person or application, which is using the virtual server, having different permissions. Investigate which is the most suitable way for you to manage users and permissions.

Another good practice is to add users to your {{site.data.keyword.cloud}} account and grant permissions to
them. This method provides the added users with capabilities within {{site.data.keyword.cloud}}. For example, with specially granted permissions, a user can view the virtual server dashboards or manage (for example, delete)
virtual servers in specific resource groups or within your account.

Detailed guidelines and tutorials for handling users and permissions are documented in topic
[Managing identity and access](https://cloud.ibm.com/docs/iam?topic=iam-getstarted).
Consider following guidelines:

* **Enable user access to the resources in your account by assigning Cloud IAM roles.** -
Rather than sharing your administrator credentials, create new policies for users who
need access to virtual servers in your accounts within IBM.

* **Grant roles and permissions at the smallest scope needed.** -
For example, if a user needs to access only information about virtual servers within one resource group,
grant the **Reader** role for {{site.data.keyword.hpvs}} to the user for that resource
group.

* **Regularly audit who has access and what permissions are provided.** -
Granting too much permissions might unintentionally allow someone else to
provide access to other users, or to delete and create resources.

## Roles and permissions

With {{site.data.keyword.cloud}} Identity and Access Management (IAM), you can manage and define access for
users and resources in your account.

To simplify access, {{site.data.keyword.hpvs}} aligns with Cloud IAM roles so that each user
has a different view of the service, according to the role the user is assigned.

The following table shows how identity and access roles map to {{site.data.keyword.hpvs}}
permissions:

| Service access role | Description | Action |
|-------|-----------------------|-------------|
| Reader | A reader can perform actions required to retrieve information only. | Retrieve information about virtual servers |
| Writer | A writer can perform the same actions as a reader – plus updating virtual servers. | Retrieve information about virtual servers |
| Manager | A manager can perform the same actions as a writer. | Retrieve information about virtual servers |
