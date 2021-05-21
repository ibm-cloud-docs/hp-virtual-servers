---

copyright:
  years: 2021
lastupdated: "2021-05-19"

subcollection: hp-virtual-servers

keywords: image, virtual server instance, instance, virtual server
---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Protecting your image build
{: #imagebuild}

You can securely build your own image, which you can then use with [bring your own image](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi).
{:shortdesc}

To do this you use the {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} Secure Build Server (SBS) functionality to sign your applications and also to sign and
encrypt the registration definition for the deployment.

When you build your image securely, you can validate your build code and reassure your users of the integrity level of their applications.

## Deploying the Secure Build Server as a Hyper Protect Virtual Server
{: #deploysecurebuild}

To deploy the SBS:

1. Install the [CLI and the hpvs plug-in](https://cloud.ibm.com/docs/hpvs-cli-plugin).
2. Copy the following registration definition for the SBS into a file named `secure_build.asc`. This file is an encrypted and signed registration definition file. It contains metadata such as the repository name and the credentials to pull and deploy the SBS image on a {{site.data.keyword.hpvs}} instance:
	```
	-----BEGIN PGP MESSAGE-----

  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
  +xNdNKDi89zrMUrBRFGXIA==
  =dNQP
	-----END PGP MESSAGE-----
	```
	{: codeblock}
3. Follow the steps in these [instructions](https://github.com/ibm-hyper-protect/secure-build-cli) to provision a Hyper Protect Virtual Server that uses the SBS and to perform your first build.
