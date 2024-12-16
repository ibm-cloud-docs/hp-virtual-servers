---

copyright:
  years:  2019, 2024
lastupdated: "2024-12-16"

subcollection: hp-virtual-servers

keywords: IAM access for {{site.data.keyword.hpvs}}, permissions for {{site.data.keyword.hpvs}}, identity and access management for {{site.data.keyword.hpvs}}, roles for {{site.data.keyword.hpvs}}, actions for {{site.data.keyword.hpvs}}, assigning access for {{site.data.keyword.hpvs}}

---

{{site.data.keyword.attribute-definition-list}}


# Managing access for {{site.data.keyword.hpvs}}
{: #iam-hpvs}

{{site.data.keyword.hpvs}} is deprecated. As of 18 August 2024, you can’t create new instances, and access to free instances will be removed. Existing premium plan instances are supported until 31 January 2025. Any instances that still exist on that date will be deleted.
{: deprecated}

The {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} service provides you with virtual servers on a highly secure stack with an Ubuntu Linux operating system.
{: shortdesc}

Access to {{site.data.keyword.hpvs}} service instances for users in your account is controlled by {{site.data.keyword.Bluemix_notm}} [Identity and Access Management (IAM)](https://cloud.ibm.com/docs/account?topic=account-iamoverview){: external}. Every user that accesses the {{site.data.keyword.hpvs}} service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.Bluemix_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. The options include the following policies:

* Access across all instances of the service in your account
* Access to an individual service instance in your account
* Access to a specific resource within an instance

After you define the scope of the access policy, you assign a role, which determines the user's level of access.
The following table details actions that are mapped to service access roles. Service access roles enable users access to {{site.data.keyword.hpvs}} and the ability to call the {{site.data.keyword.hpvs}} API.

| Service access role | Description | Action |
|-------|-----------------------|-------------|
| Reader | A reader can perform actions that are required to retrieve information only. | Retrieve information about virtual servers. |
| Writer | A writer can perform the same actions as a reader – plus updating virtual servers. | Retrieve information about virtual servers. |
| Manager | A manager can perform the same actions as a writer. | Retrieve information about virtual servers. |
{: caption="IAM service access roles and actions" caption-side="bottom"}

Consider the following guidelines:

* **Enable user access to the resources in your account by assigning Cloud IAM roles.** -

Rather than sharing your administrator credentials, create new policies for users who
need access to virtual servers in your accounts within IBM.

* **Grant roles and permissions at the smallest scope needed.** -

For example, if a user needs to access only information about virtual servers within one resource group,
grant the **Reader** role for {{site.data.keyword.hpvs}} to the user for that resource
group.

* **Regularly audit who has access and what permissions are provided.** -

Granting too many permissions might unintentionally allow someone else to
provide access to other users, or to delete and create resources.

For information about assigning user roles in the console, see [Managing access to resources](https://cloud.ibm.com/docs/account?topic=account-assign-access-resources).
