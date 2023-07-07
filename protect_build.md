---

copyright:
  years: 2021, 2023
lastupdated: "2022-07-07"

subcollection: hp-virtual-servers

keywords: image, virtual server instance, instance, virtual server
---


{{site.data.keyword.attribute-definition-list}}

# Protecting your image build
{: #imagebuild}

You can now use Hyper Protect Secure Build to build a trusted container image within a secure enclave that is provided by {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC. For more information, see [Hyper Protect Secure Build](/docs/vpc?topic=vpc-about-hpsb).
{: tip}

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
      Version: GopenPGP 2.5.0

      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
      =c6rC
      -----END PGP MESSAGE-----
      ```
      {: codeblock}


3. Follow the steps in these [instructions](https://github.com/ibm-hyper-protect/secure-build-cli/blob/master/SBS-HPVScloud.md) to provision a Hyper Protect Virtual Server that uses the SBS and to perform your first build.
