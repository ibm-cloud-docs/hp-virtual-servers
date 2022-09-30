---

copyright:
  years: 2019, 2022
lastupdated: "2022-09-30"

keywords: release note, what's new

subcollection: hp-virtual-servers

content-type: release-note

---


{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}

# Release notes
{: #whats-new}

Use the release notes to learn the latest updates to {{site.data.keyword.hpvs}}.
{: shortdesc}

## 30 September 2022
{: #hp-virtual-servers-sep3022}
{: release-note}

Updated: Secure Build Server version
:   The Secure Build Server image is updated to 1.3.0.7. For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild), and [Tutorial: Using Secure Build Server with a digital wallet](/docs/hp-virtual-servers?topic=hp-virtual-servers-tutorial_secure_build_server).

## 30 June 2022
{: #hp-virtual-servers-jun3022}
{: release-note}

Updated: Secure Build Server version
:   The Secure Build Server image is updated to 1.3.0.6. For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild), and [Tutorial: Using Secure Build Server with a digital wallet](/docs/hp-virtual-servers?topic=hp-virtual-servers-tutorial_secure_build_server).

## 31 March 2022
{: #hp-virtual-servers-mar3122}
{: release-note}

Updated: Tutorial on setting up the secure network
:   The secure network is updated to version 2.0.0. You can now use the CLI tool as an {{site.data.keyword.cloud}} CLI plugin. All actions are recorded as audit logs and are forwarded to LogDNA. For more information, see [Tutorial: Setting up the secure network](/docs/hp-virtual-servers?topic=hp-virtual-servers-tutorial_network).


## 15 March 2022
{: #hp-virtual-servers-mar1522}
{: release-note}

Updated: Secure Build Server version
:   The Secure Build Server image is updated to 1.3.0.5. For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild), and [Tutorial: Using Secure Build Server with a digital wallet](/docs/hp-virtual-servers?topic=hp-virtual-servers-tutorial_secure_build_server).

## 13 December 2021
{: #hp-virtual-servers-dec1321}
{: release-note}

Updated: The repository definition file.
:   The repository definition file is updated. For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild).

Updated: Secure Build Server version
:   The Secure Build Server image is updated to 1.3.0.4, and the Secure Build CLI code is also updated. The security of the communication between the SBS server and client has been enhanced to prevent malicious attacks. Ensure that you update to the latest Secure Build CLI code and also use the latest `secure_build.asc` file. For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild), and [Tutorial: Using Secure Build Server with a digital wallet](/docs/hp-virtual-servers?topic=hp-virtual-servers-tutorial_secure_build_server).

## 08 December 2021
{: #hp-virtual-servers-dec0821}
{: release-note}

Updated: The 'hpvs instance-update' command now supports the '--hostname' parameter.
:   You can now use the `--hostname` parameter when you update a {{site.data.keyword.hpvs}} instance. For more information, see [IBM Cloud Hyper Protect Virtual Servers CLI](/docs/hp-virtual-servers?topic=hpvs-cli-plugin-hpvs_cli_plugin#hpvsinstanceupdate).


## 17 November 2021
{: #hp-virtual-servers-nov1721}
{: release-note}

Updated: The 'hpvs instance-create' command now supports the '--hostname' parameter.
:   You can now use the `--hostname` parameter to specify the hostname that will be set within the Hyper Protect Virtual Servers container. For more information, see [IBM Cloud Hyper Protect Virtual Servers CLI](/docs/hp-virtual-servers?topic=hpvs-cli-plugin-hpvs_cli_plugin#create_instance), and [Provisioning a virtual server](/docs/hp-virtual-servers?topic=hp-virtual-servers-provision#provision-cli).

Updated: The 'hpvs registration-create' command now supports the '--isv-secrets' and '--no-isv-secrets' parameters.
:   You can now use the `--isv-secrets` or `--no-isv-secrets` parameters. For more information, see [IBM Cloud Hyper Protect Virtual Servers CLI](/docs/hp-virtual-servers?topic=hpvs-cli-plugin-hpvs_cli_plugin#hpvsregistrationfilecreate), and [Using your own image](/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_regdef_cli).   

Updated: The length of the environment variable value.
:   You can now specify environment variable values that can have a maximum length of 12288. For more information, see [IBM Cloud Hyper Protect Virtual Servers CLI](/docs/hp-virtual-servers?topic=hpvs-cli-plugin-hpvs_cli_plugin#create_instance).   


## 30 September 2021
{: #hp-virtual-servers-sep3021}
{: release-note}

Updated: The hpvs cli is now supported on s390x.
:   The hpvs cli is now supported on the IBM Z platform (s390x architecture). For more information, see [IBM Cloud Hyper Protect Virtual Servers CLI](/docs/hp-virtual-servers?topic=hpvs-cli-plugin-hpvs_cli_plugin).


## 19 August 2021
{: #hp-virtual-servers-aug1921}
{: release-note}

Updated: Ubuntu Linux 20.04 is now supported.
:   The provided Ubuntu operating system is now partwisely hardened according to CIS Ubuntu Linux 20.04 LTS Benchmark, Level 1 - Server profile. This operating system update will be provided only for new instances.

## 29 July 2021
{: #hp-virtual-servers-jul2921}
{: release-note}

Added: Secure network tutorial
:   The tutorial "Secure network" is now available that describes how you can set up the secure network which provides an end to end encrypted network communication for IBM Cloud {{site.data.keyword.hpvs}} services. For more information, see [Tutorial: Setting up the secure network](/docs/hp-virtual-servers?topic=hp-virtual-servers-tutorial_network).

## 18 June 2021
{: #hp-virtual-servers-jun1821}
{: release-note}

Updated: The repository definition file.
:   The repository definition file is updated to pull images from the IBM Cloud Registry. For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild).

## 16 June 2021
{: #hp-virtual-servers-jun1621}
{: release-note}

Updated: The `hpvs registration-create` command.
:   You can now add Linux capabilities by using the `hpvs registration-create` command. For more information, see [Using your own image](/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi).

## 15 June 2021
{: #hp-virtual-servers-jun1521}
{: release-note}

Added: Monitoring logs of {{site.data.keyword.hpvs}} instances.
:   You can monitor many kinds of logs of the {{site.data.keyword.hpvs}} instances by using IBM Log Analysis with LogDNA on the {{site.data.keyword.hpvs}} instance. For more information, see [Monitoring logs](/docs/hp-virtual-servers?topic=hp-virtual-servers-monitoring).

## 21 May 2021
{: #hp-virtual-servers-may2121}
{: release-note}

Updated: Deploying the Secure Build Server as a Hyper Protect Virtual Server.
:   The {{site.data.keyword.cloud_notm}} command line interface can be used to create a registration definition file, and the gpg registration key. You can also manually add Linux capabilities to the registration definition file. The registration definition file for SBS is updated for security compliance.  For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild).

## 26 February 2021
{: #hp-virtual-servers-feb2621}
{: release-note}

Added: {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} Secure Build Server feature.
:   You can now use {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} Secure Build Server to securely build an image, which you then can use to provision a virtual server.  For more information, see [Deploying the Secure Build Server as a Hyper Protect Virtual Server](/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild#deploysecurebuild).

Added: The tutorial about using the Secure Build Server with a digital wallet
:   The tutorial "Using Secure Build Server with a digital wallet" is available. For more information, see [Tutorial: Using Secure Build Server with a digital wallet](/docs/hp-virtual-servers?topic=hp-virtual-servers-tutorial_secure_build_server).


## 24 February 2021
{: #hp-virtual-servers-feb2421}
{: release-note}

The new parameter `outbound` is now available when you provision a server.


## 12 October 2020
{: #hp-virtual-servers-oct1220}
{: release-note}

You can provide environment variables when you provision and update servers using a custom OCI image.

## 15 September 2020
{: #hp-virtual-servers-sep1520}
{: release-note}

You can now update the OCI image that's used for your virtual server.

## 08 August 2020
{: #hp-virtual-servers-aug0820}
{: release-note}

The {{site.data.keyword.hpvs}} command line interface is upgraded to v1.0.1. to include security and other minor fixes together with improved error messages.

## 27 July 2020
{: #hp-virtual-servers-jul2720}
{: release-note}

The new {{site.data.keyword.hpvs}} command line interface is now available.


## 06 July 2020
{: #hp-virtual-servers-jul0620}
{: release-note}

Three new data centers are available for creating virtual servers: Washington 04, Washington 06, and Washington 07.


## 14 April 2020
{: #hp-virtual-servers-apr1420}
{: release-note}

Detailed information about enhancing security for {{site.data.keyword.hpvs}} is now available.

## 26 March 2020
{: #hp-virtual-servers-mar2620}
{: release-note}

The {{site.data.keyword.cloud_notm}} command line interface can be used to list, create, or delete your {{site.data.keyword.hpvs}} instances.


## 24 March 2020
{: #hp-virtual-servers-mar2420}
{: release-note}

A virtual server now has a seven day reclamation period after you "delete" it from the resource list. During the reclamation period you can restore the virtual server or manually trigger a deletion via resource reclamations.

## 27 February 2020
{: #hp-virtual-servers-feb2720}
{: release-note}

A new pricing plan is available: **Entry**.

## 30 January 2020
{: #hp-virtual-servers-jan3020}
{: release-note}

Three new data centers are available for creating virtual servers: Frankfurt 02, Frankfurt 04, and Frankfurt 05.

## 19 December 2019
{: #hp-virtual-servers-dec1919}
{: release-note}

Instances of the "Free" plan can now be provisioned in Dallas 10 and Dallas 12.

## 17 December 2019
{: #hp-virtual-servers-dec1719}
{: release-note}

The provided Ubuntu operating system is now partwisely hardened according to CIS Ubuntu Linux 18.04 LTS Benchmark, Level 1 - Server profile.
This operating system update will only be provided to new instances.

## 12 December 2019
{: #hp-virtual-servers-dec1219}
{: release-note}

The service offering moved from the **Beta** to the **GA** phase. Additionally, new pricing plans are available: **Medium** and **Small**.

## 6 December 2019
{: #hp-virtual-servers-dec619}
{: release-note}

Three new data centers are available for creating virtual servers: Sydney 01, Sydney 04, and Sydney 05.

## 12 November 2019
{: #hp-virtual-servers-nov1219}
{: release-note}

You can now order virtual servers in different data centers: Dallas 10, Dallas 12, and Dallas 13.

## 21 October 2019
{: #hp-virtual-servers-oct2119}
{: release-note}

Your virtual server instances are now isolated in one virtual LAN (VLAN) per account and region.


## 30 August 2019
{: #hp-virtual-servers-aug3019}
{: release-note}

The service offering moved from the **Experimental** to the **Beta** phase.

## 16 August 2019
{: #hp-virtual-servers-aug1619}
{: release-note}

A new and extended version of the documentation is available.

## 12 February 2019
{: #hp-virtual-servers-feb19}
{: release-note}

The **Experimental** version of {{site.data.keyword.hpvs}} is released. You can now access the {{site.data.keyword.hpvs}} service in the {{site.data.keyword.cloud_notm}} Catalog under the **Compute** category.
