---

copyright:
  years: 2019, 2024
lastupdated: "2024-12-16"

subcollection: hp-virtual-servers

---

{{site.data.keyword.attribute-definition-list}}

# Solving problems
{: #troubleshooting}

{{site.data.keyword.hpvs}} is deprecated. As of 18 August 2024, you can’t create new instances, and access to free instances will be removed. Existing premium plan instances are supported until 31 January 2025. Any instances that still exist on that date will be deleted.
{: deprecated}

If you run into problems while you're using {{site.data.keyword.hpvs}}, you can recover from these problems in many cases by following a few steps.
{: shortdesc}

## Installion of the {{site.data.keyword.cloud_notm}} CLI fails
{: #install_cli_hpvs}
{: troubleshoot}
{: support}

I can't install the {{site.data.keyword.cloud_notm}} CLI on the virtual server.
{: troubleshoot}

You see an error message indicating that you do not have the permission to install the CLI.
{: tsSymptoms}

This error occurs because the `/tmp` file system does not have execute permission.
{: tsCauses}

Run the `mount -i -o remount,exec /tmp/` command to remount the `/tmp` file system with execute permission.
{: tsResolve}


## You can't see the details of your virtual server instance on its dashboard - or the dashboard seems to display incorrectly
{: #no_details_hpvs}
{: troubleshoot}
{: support}

Though a new virtual server was successfully created, the details of the system are displayed incorrectly or aren't displayed at all.
{: troubleshoot}

The dashboard of your new virtual server does not show configuration details of your system, or these details aren't presented in the correct manner.
{: tsSymptoms}

This issue can have multiple causes. The most probable causes are either a caching issue, or you might be using an unsupported browser.
{: tsCauses}

Verify that you're using a supported browser. Delete your browser cache, log out of the {{site.data.keyword.cloud_notm}}, and log in again.
{: tsResolve}

## Status 'Inactive' is displayed in the **Resource list** when you create multiple virtual servers
{: #provision_failure}
{: troubleshoot}
{: support}

If you create more than five virtual server instances almost simultaneously in one data center that has no  existing instances, one or more virtual servers displays with the status 'Inactive' (provisioning failed) within the **Resource list**. When you click the 'Inactive' server in **Name** field, an error message is displayed to indicate that provisioning has failed.
{: troubleshoot}

You try to create more than five virtual servers within a short period in one data center without any existing virtual server instances.
{: tsSymptoms}

The number of virtual servers per data center is limited to five. Creating more than five virtual servers almost all at the same time in one data center leads to the status 'Inactive' (provisioning failed) for the sixth and all subsequent instances.
{: tsCauses}

You can either provision more than five virtual servers in different data centers. Or you can provision more than five instances by using different {{site.data.keyword.cloud_notm}} accounts.
{: tsResolve}


##  Generating a new virtual server fails because of exhausted resources
{: #max_number_resources}
{: troubleshoot}
{: support}

You can only create a limited number of virtual server instances in each data center.
{: troubleshoot}

When you provision a new virtual server, you get an error message because the resources (for example, memory) are exhausted.
{: tsSymptoms}

The resources for the selected data center are exhausted.
{: tsCauses}

Retry the action and select a different data center.
{: tsResolve}


## Can't connect to free virtual server anymore
{: #connect_freevs}
{: troubleshoot}
{: support}

I can't connect to a server (for example, via SSH), which is in the free plan although it's visible in the {{site.data.keyword.cloud_notm}} resource list.
{: troubleshoot}

You can't connect to a server in the free plan anymore.
The server is still visible in the resource list, but the dashboard shows an error message. The message indicates that the server has expired. When a server expires, the server and all data that is stored on the server are deleted.
{: tsSymptoms}

All virtual servers in free plans are deleted after 30 days.
{: tsCauses}

Virtual servers in paid plans aren't deleted. You can remove the server from the resource list by deleting it from the resource list.
{: tsResolve}


## Can't create new virtual servers because of exhausted resources or plan limitations
{: #connect_newvs}
{: troubleshoot}
{: support}

You can't create new virtual servers because of exhausted resources or plan limitations, although you've less than the described limits.
{: troubleshoot}

You try to create a new server but fail. Instead a message is displayed, which informs you that the maximum number of servers is reached in the selected data center for this account, or that you've reached the maximum number of free plans for this account. When you look in the resource list, you see that you haven't reached the limit.
{: tsSymptoms}

The resource list is not a complete list of your servers. If you “delete” a server from the resource list, it's made inaccessible and marked for deletion, but it's still available for you within the [resource reclamations](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external} for a limited amount of time, as described in [Deleting a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-remove_vs).
{: tsCauses}

Depending on your requirements, you must either delete the resource from the resource reclamations, or select a different data center (maximum number of servers is reached), or select the appropriate paid plan that meets your requirements if you reach the maximum number of free plans.
For more information, see [resource reclamations](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}.
{: tsResolve}


## VoiceOver on MacOS does not announce all information on the dashboard in Firefox
{: #macos_voiceover}
{: troubleshoot}
{: support}

VoiceOver on MacOS does not announce information presented on the dashboard in Firefox. It does not announce the public IP address, the internal IP address, the command how to connect to a virtual server, the initial SSH public key fingerprint, the image name, the image type, the operating system.
{: troubleshoot}

You try to announce the information presented on the dashboard using VoiceOver on MacOS in Firefox. No information is announced.
{: tsSymptoms}

Firefox does not fully support assistive technology. For more information, see [Proper voiceover support coming soon](https://blog.mozilla.org/accessibility/proper-voiceover-support-coming-soon-to-firefox-on-macos/).
{: tsCauses}

Use VoiceOver with either Safari or Chrome.
{: tsResolve}
