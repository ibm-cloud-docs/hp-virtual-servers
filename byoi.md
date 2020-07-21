---

copyright:
  years: 2020
lastupdated: "2020-07-15"

subcollection: hp-virtual-servers

keywords: image, virtual server instance, instance, virtual server

---
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:important: .important}

# Using your own image
{: #byoi}

Use your own Linux-based OCI image to create a new Hyper Protect Virtual Server.
{: shortdesc}

This feature is for technical preview (experimental) only, it is currently not supported by IBM and should not be used in production environments.
{:important}

The {{site.data.keyword.cloud}} CLI can only be used on the officially supported operating systems or architectures. If your system is not supported, you could consider using IBM Cloud Shell.
{:note}

Before you provision a new server, check the [prerequisites](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_pre), then complete the following steps:

1. [Create a custom image](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_create)
2. [Create a registration definition file](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_regdef)
3. [Provision a Hyper Protect Virtual Server using your OCI Image](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_provision)


## Prerequisites
{: #byoi_pre}

- Your custom Linux-based image must be:
  - In the OCI Image format. {{site.data.keyword.hpvs}} supports only Linux-based OCI Images which are built for the IBM LinuxONE and IBM Z platform (s390x architecture)
  - Available either in the [{{site.data.keyword.cloud}} Container Registry](https://cloud.ibm.com/kubernetes/catalog/registry) or [Docker Hub](https://hub.docker.com/) and signed using [Docker Content Trust](https://docs.docker.com/engine/security/trust/content_trust/).
- You must install the {{site.data.keyword.cloud_notm}} CLI. Follow these [instructions](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started) to install and configure the {{site.data.keyword.cloud_notm}} CLI.

## Creating a custom image
{: #byoi_create}
1. Choose a base for your OCI Image. Either use one of the [publicly available images from Docker Hub](https://hub.docker.com/search?q=&type=image&image_filter=&architecture=s390x) or build one from scratch.
1. Use a build tool such as `docker build` to make your required modifications to the base. These tools can be installed on a Hyper Protect Virtual Server using Ubuntu if you don't have access to a system running on the IBM Z platform (s390x architecture) or can't cross build your image. To install Docker on your Hyper Protect Virtual Server run `apt update && apt install -y docker.io`. Be aware of these hints when you build your image:
    - The maximum allowed image size is 5 GB.
    - Set the entrypoint and command used to start the OCI image during the image build because these parameters can’t be configured afterwards to provision your virtual server.
    - {{site.data.keyword.cloud}} Registry Namespaces and Docker Hub organizations with a `-` in the name are not permitted.
    - The OCI image content is available on the virtual server boot disk mounted as `/`. If you need to use persistent storage use the `/data` mountpoint.
    - If you want to use `systemd` in your image, make sure you set the start command to `/sbin/init` and configure the environment variable `RUNQ_SYSTEMD=1`.
    - The virtual server gets its own IP address which means that all open ports are automatically mapped on the internal and public network. Make sure you restrict network access in your image to only the ports you require. `EXPOSE` rules from the image build don't have any effect.
    - The image must be tagged according to the following format to later “push” the image to the registry:
     ` <registry url>/<namespace>/<name>:<tag>`
     where:
      - registry url: Within ICR: it could be, for example `de.icr.io`, for DockerHub: `docker.io`.
      - namespace: Is the [namespace created](https://cloud.ibm.com/docs/Registry?topic=Registry-getting-started#gs_registry_namespace_add) in the {{site.data.keyword.cloud_notm}} Container Registry, see also step 4 of this procedure.
      - name: Is the unique name for the repository (one repository can have multiple images, that differ by the tag).
      - tag: Is the unique tag for the image.

1. After you have created your image, check that there are no vulnerable components. Most containers are built using a collection of open source components that can suffer from known vulnerabilities. It is your responsibility to use scanning tools to identify if any of the components you have included in the build are vulnerable. Scan the components before you distribute the image, for example, with [Vulnerability Advisor](https://cloud.ibm.com/docs/Registry?topic=va-va_index).
1. Push the image to either the {{site.data.keyword.cloud_notm}} Registry or Docker Hub with Docker Content Trust enabled. To do this using the {{site.data.keyword.cloud_notm}} Container Registry follow these steps:
    - Follow the [instructions to create an API key](https://cloud.ibm.com/docs/iam?topic=iam-userapikey#create_user_key)
    - Follow the [instructions to create a namespace](https://cloud.ibm.com/docs/Registry?topic=Registry-getting-started#gs_registry_namespace_add) in {{site.data.keyword.cloud_notm}} Container Registry.
    - To log in to the registry on your terminal, run:
      ```
      echo "<API_Key>" | docker login -u "iamapikey" --password-stdin <registry_region>.icr.io
      ```
      {: pre}
    - Push the image by running:
      ```
      DOCKER_CONTENT_TRUST=1 DOCKER_CONTENT_TRUST_SERVER=https://<registry_region>.icr.io:4443 docker push <image>
      ```
      {: pre}

1. Follow these [back up instructions](https://cloud.ibm.com/docs/Registry?topic=registry-registry_trustedcontent#trustedcontent_backupkeys) to back up your keys after you have pushed the image.


## Creating a registration definition file
{: #byoi_regdef}
The registration definition file contains metadata about the OCI image you want to use for your Hyper Protect Virtual Server, such as the repository name and the credentials to pull the image. Because it can contain secret information you must encrypt and sign it before providing it to create a new Virtual Server. You can do this on any system that has `gpg` installed. To use a Hyper Protect Virtual Server to do this, install the `gpg` package by running `apt update && apt install -y gpg`.

1. Create a file and copy this template for registration definition files into it:
  ```
  {
        "repository_name": "",
        "docker_username": "",
        "docker_password": "",
        "envs_whitelist": ["RUNQ_ROOTDISK", "RUNQ_RUNQENV", "RUNQ_SYSTEMD", "IMAGE_TAG", "REGION", "PHASE", "LPAR_NAME", "CPC", "RUNQ_CPU", "RUNQ_MEM", "POD"],
        "public_key_id": "",
        "public_key": "",
        "vendor_key": ""
  }
  ```
  {: codeblock}
1. Enter the value for the repository name in the `repository_name` field in the template. This parameter requires the full path to the OCI image without the tag, for example, `docker.io/library/ubuntu/` or `de.icr.io/hpvs/ubuntu`.
1. Enter your credentials to authenticate on the registry in the `docker_username` and `docker_password` fields. If you use {{site.data.keyword.cloud_notm}} Container Registry your user name is `iamapikey` and the password is your API key. If your image does not require authentication leave the values for `docker_username` and `docker_password` empty.
1. Run `docker trust inspect <image>` and copy the Root key ID from the AdministrativeKeys that was used to sign the Docker Image on your build system. Paste the ID into the `public_key_id` value field in the template. For example:
  ```
  DOCKER_CONTENT_TRUST_SERVER=https://<registry_region>.icr.io:4443 docker trust inspect <image>
  ```
  {: pre}
  ```
  [
      {
          "Name": "<image>",
          "SignedTags": [
              {
                  "SignedTag": "<tag>",
                  "Digest": "1e8d9dee88b7db28057ade6b870ef40f98bc67492588568520fa2ba5000b8fa0",
                  "Signers": [
                      "Repo Admin"
                  ]
              }
          ],
          "Signers": [],
          "AdministrativeKeys": [
              {
                  "Name": "Root",
                  "Keys": [
                      {
                          "ID": "c4a4088f868d2fd6f11686f7851e7166d7ef3e61c002ee990f5c68c21b038601"
                      }
                  ]
              },
              {
                  "Name": "Repository",
                  "Keys": [
                      {
                          "ID": "1b162fbf73a5baa9e59174b863a650a09a8752e89f67fc0548f83238183b1934"
                      }
                  ]
              }
          ]
      }
  ]
  ```
1. Open the Docker metadata file `~/.docker/trust/tuf/<image>/metadata/root.json` and locate the Key ID from the previous step. Copy the public key from that file and paste it in the `public_key` field. With that step the content of the template is complete.
1. Create a gpg batch file with the following content and add your own passphrase:
  ```
  %echo Generating registration definition key
  Key-Type: RSA
  Key-Length: 4096
  Subkey-Type: RSA
  Subkey-Length: 4096
  Name-Real: isv_user
  Expire-Date: 0
  Passphrase: <passphrase>
  # Do a commit here, so that we can later print "done" :-)
  %commit
  %echo done
  ```
  {: codeblock}

1. Run `gpg -a --batch --generate-key <batchfile>` to create a new keypair as the `isv_user` identity which will be used for signing the registration file in a later step:
  ```
  gpg -a --batch --generate-key <batchfile>
  ```
  {: pre}
  ```
  gpg: Generating registration definition key
  gpg: /root/.gnupg/trustdb.gpg: trustdb created
  gpg: key DDEC881AEE6DD1F5 marked as ultimately trusted
  gpg: directory '/root/.gnupg/openpgp-revocs.d' created
  gpg: revocation certificate stored as '/root/.gnupg/openpgp-revocs.d/12B9EF207977393E334DDAD1DDEC881AEE6DD1F5.rev'
  gpg: done
  ```
1. Export the public part of the key by running `gpg --armor --export isv_user > isv_user.pub`
  ```
  gpg --armor --export isv_user > isv_user.pub
  ```
  {: pre}
1. To back up your private key, you can export it by running `gpg --armor --pinentry-mode=loopback --passphrase  <passphrase> --export-secret-keys isv_user > isv_user.private`. Store the exported private key somewhere else.
  ```
  gpg --armor --pinentry-mode=loopback --passphrase  <passphrase> --export-secret-keys isv_user > isv_user.private
  ```
  {: pre}
1. Open the isv_user.pub and replace all newlines with `\n`. You can do this in vim, by running `:%s/\n/\\n/g`. Copy the complete content of the file and paste it in the `vendor_key` value in the template.
1. Copy the following public key, which will be used for encrypting the registration file, into a file:
  ```
  -----BEGIN PGP PUBLIC KEY BLOCK-----
  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
  mae7WLtydkQnM7Shc1l2CmBCHH8ClSJN
  =DsnN
  -----END PGP PUBLIC KEY BLOCK-----
  ```
  {: codeblock}
1. To import the public key from the previous step, run:  
  ```
  gpg --import <filename>
  ```
  {: pre}

1. To encrypt and sign the registration definition file, run:
  ```
  gpg --encrypt --sign --local-user isv_user --armor -r rtoa_destination <registration_template>
  ```
  {: pre}

  The signature is done with the private key you generated earlier and the file is encrypted using the public key you just imported.


## Provisioning a Hyper Protect Virtual Server using your OCI Image
{: #byoi_provision}
You must use the [CLI](https://cloud.ibm.com/docs/hpvs-cli-plugin) `ibmcloud hpvs instance-create` command to provision a Hyper Protect Virtual Server using your own OCI image, as described [here](https://test.cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-provision#provision-cli).

### Example
{: #byoi_example}

```
ibmcloud hpvs instance-create myServerName lite-s dal13 --rd-path "C:\MyRegistrationDefinitions\registration-def.json.asc" -i latest
```
