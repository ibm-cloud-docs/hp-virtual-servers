---

copyright:
  years: 2021
lastupdated: "2021-12-13"

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

To do this you use the {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} Secure Build Server (SBS) functionality to sign your applications and also to sign and
encrypt the registration definition for the deployment.

When you build your image securely, you can validate your build code and reassure your users of the integrity level of their applications.

## Deploying the Secure Build Server as a Hyper Protect Virtual Server
{: #deploysecurebuild}

To deploy the SBS:

1. Install the [CLI and the hpvs plug-in](https://cloud.ibm.com/docs/hpvs-cli-plugin).
2. Copy the following registration definition for the SBS into a file named `secure_build.asc`. This file is an encrypted and signed registration definition file. It contains metadata such as the repository name and the credentials to pull and deploy the SBS image on a {{site.data.keyword.hpvs}} instance:
      ```sh
      -----BEGIN PGP MESSAGE-----
      Version: GopenPGP 2.3.0
      Comment: https://gopenpgp.org

      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
      =5nky
      -----END PGP MESSAGE-----
      ```
      {: codeblock}


3. Follow the steps in these [instructions](https://github.com/ibm-hyper-protect/secure-build-cli) to provision a Hyper Protect Virtual Server that uses the SBS and to perform your first build.
