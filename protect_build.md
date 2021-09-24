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

      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
      =hnv4
      -----END PGP MESSAGE-----
      ```
     {: codeblock}


3. Follow the steps in these [instructions](https://github.com/ibm-hyper-protect/secure-build-cli) to provision a Hyper Protect Virtual Server that uses the SBS and to perform your first build.
