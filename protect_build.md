---

copyright:
  years: 2021, 2022
lastupdated: "2022-12-07"

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

You can securely build your own image, which you can then use with [bring your own image](/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi).
{: shortdesc}

To do this you use the {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} Secure Build Server (SBS) functionality to sign your applications and also to sign and encrypt the registration definition for the deployment.

When you build your image securely, you can validate your build code and reassure your users of the integrity level of their applications.

## Deploying the Secure Build Server as a Hyper Protect Virtual Server
{: #deploysecurebuild}

To deploy the SBS:

1. Install the [CLI and the hpvs plug-in](https://cloud.ibm.com/docs/hpvs-cli-plugin).
2. Copy the following registration definition for the SBS into a file named `secure_build.asc`. This file is an encrypted and signed registration definition file. It contains metadata such as the repository name and the credentials to pull and deploy the SBS image on a {{site.data.keyword.hpvs}} instance:
      ```sh
      -----BEGIN PGP MESSAGE-----
      Comment: https://gopenpgp.org
      Version: GopenPGP 2.4.10

      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
      2j2/HRGHjl5K
      =9HfX
      -----END PGP MESSAGE-----
      ```
      {: codeblock}


3. Follow the steps in these [instructions](https://github.com/ibm-hyper-protect/secure-build-cli) to provision a Hyper Protect Virtual Server that uses the SBS and to perform your first build.
