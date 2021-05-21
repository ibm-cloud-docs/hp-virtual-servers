---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-19"

subcollection: hp-virtual-servers

keywords: image, virtual server instance, instance, virtual server

---
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:note: .note}
{:tip: .tip}
{:important: .important}

# Using your own image
{: #byoi}

Use your own Linux-based OCI image to create a new Hyper Protect Virtual Server.
{: shortdesc}


{:note}
The {{site.data.keyword.cloud}} CLI can be used only on the officially supported operating systems or architectures. It is recommended that you use the {{site.data.keyword.cloud}} Shell if your system is not supported.

If you want to use {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} Secure Build Server to provision a server, refer to [Protecting your image build](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-imagebuild).

{:note}
Do not use personal information, for example, your name, as the instance name or as part of the instance name. The data that you provide when you provision an instance or interact with the hpvs cli is not considered to be personal data or credentials.
Learn more about IBM Cloud Hyper Protect Virtual Servers' Data usage and Certifications [here](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=165C6EF0FFDA11E8BABD512A6952EE1F).

Before you provision a new server, check the [prerequisites](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_pre), then complete the following steps:

1. [Create a custom image](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_create).
2. [Adding Linux capabilities to the registration definition file](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_regdef).
3. [Creating a registration definition file by using the CLI](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_regdef_cli).  
4. [Use your OCI image to provision a Hyper Protect Virtual Server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_provision).


## Prerequisites
{: #byoi_pre}

- Your custom Linux-based image must meet these requirements:
  - In the OCI Image format. {{site.data.keyword.hpvs}} supports only Linux-based OCI Images, which are built for the IBM LinuxONE and IBM Z platform (s390x architecture)
  - Available either in the [{{site.data.keyword.cloud}} Container Registry](https://cloud.ibm.com/kubernetes/catalog/registry) or [Docker Hub](https://hub.docker.com/) and signed by using [Docker Content Trust](https://docs.docker.com/engine/security/trust/content_trust/).
- You must install the {{site.data.keyword.cloud_notm}} CLI. Follow these [instructions](https://cloud.ibm.com/docs/cli?topic=cli-getting-started) to install and configure the {{site.data.keyword.cloud_notm}} CLI.

## Creating a custom image
{: #byoi_create}
1. Choose a base for your OCI Image. Either use one of the [publicly available images from Docker Hub](https://hub.docker.com/search?q=&type=image&image_filter=&architecture=s390x) or build one from scratch.
2. Use a build tool such as `docker build` to make your required modifications to the base. You can use Ubuntu to install these tools on a Hyper Protect Virtual Server if you don't have access to a system that runs on the IBM Z platform (s390x architecture), or you can't cross build your image. To install Docker on your Hyper Protect Virtual Server, run `apt update && apt install -y docker.io`. Be aware of these hints when you build your image:
    - The maximum allowed image size is 5 GB.
    - Set the entrypoint and command that is used to start the OCI image during the image build because these parameters can’t be configured afterward to provision your virtual server.
    - {{site.data.keyword.cloud}} Registry namespaces and Docker Hub organizations with a `-` in the name are not permitted.
    - The OCI image content is available on the virtual server boot disk that is mounted as `/`. If you need to use persistent storage, use the `/data` mount point.
    - If you want to use `systemd` in your image, make sure you set the start command to `/sbin/init` and configure the environment variable `RUNQ_SYSTEMD=1`.
    - The virtual server gets its own IP address, which means that all open ports are automatically mapped on the internal and public network. Make sure you restrict network access in your image to only the ports you require. `EXPOSE` rules from the image build are not applied.
    - The image must be tagged according to the following format to later “push” the image to the registry:
     ` <registry url>/<namespace>/<name>:<tag>`
     where:
      - registry url: Within ICR: An example is `de.icr.io`, or for DockerHub: `docker.io`.
      - namespace: Is the [namespace created](https://cloud.ibm.com/docs/Registry?topic=Registry-getting-started#gs_registry_namespace_add) in the {{site.data.keyword.cloud_notm}} Container Registry, see also step 4 of this procedure.
      - name: Is the unique name for the repository (one repository can have multiple images that differ by the tag).
      - tag: Is the unique tag for the image.
3. After you create your image, check for vulnerable components. Most containers are built by using a collection of open source components that can suffer from known vulnerabilities. It is your responsibility to use scanning tools to identify if any of the components you include in the build are vulnerable. Scan the components before you distribute the image, for example, with [Vulnerability Advisor](https://cloud.ibm.com/docs/Registry?topic=va-va_index).
4. Push the image to either the {{site.data.keyword.cloud_notm}} Registry or Docker Hub with Docker Content Trust enabled. To do this using the {{site.data.keyword.cloud_notm}} Container Registry, follow these steps:
    - Follow the [instructions to create an API key](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key).
    - Follow the [instructions to create a namespace](https://cloud.ibm.com/docs/Registry?topic=Registry-getting-started#gs_registry_namespace_add) in {{site.data.keyword.cloud_notm}} Container Registry.
    - To log in to the registry on your console, run:
      ```
      echo "<API_Key>" | docker login -u "iamapikey" --password-stdin <registry_region>.icr.io
      ```
      {: pre}
    - Push the image by running:
      ```
      DOCKER_CONTENT_TRUST=1 DOCKER_CONTENT_TRUST_SERVER=https://<registry_region>.icr.io:4443 docker push <image>
      ```
      {: pre}

5. Follow the [backup instructions](https://cloud.ibm.com/docs/Registry?topic=Registry-registry_trustedcontent#trustedcontent_backupkeys) to back up your keys after you push the image.


## Creating a registration definition file by using the CLI
{: #byoi_regdef_cli}
The registration definition file contains metadata about the OCI image you want to use for your Hyper Protect Virtual Server, such as the repository name and the credentials to pull the image. Because it can contain secret information, the file is  automatically encrypted and signed by the CLI before you can use it to create a new Virtual Server. Use the following commands to create an encrypted registration definition file.

Before you call the `hpvs registration-key-create` command, `gpg` must be installed on your system.

1. To create a `gpg` registration key, run the `hpvs registration-key-create` command. The resulting output files are the required inputs for the `hpvs registration-create` command.

  ```
  ibmcloud hpvs registration-key-create ID [--gpg-passphrase-path FILE-PATH] [-v VERBOSE]
  ```
  {: pre}

  Where:
  <dl>
  <dt>`ID`</dt>
  <dd>Is the user ID to set for the gpg registration key.</dd>
  <dt>`--gpg-passphrase-path FILE-PATH` </dt>
  <dd>Is the path for the file that contains the passphrase for the registration key. The passphrase must consist of at least 6 characters. To make sure that a new line is not appended, use `echo` with `-n` or cat with EOF. If the path is not specified, you are prompted for the passphrase.</dd>
  <dt>`-v, --verbose`</dt>
  <dd>Set to true for verbose output.</dd>
  </dl>

2. To create a registration file, run the `hpvs registration-create` command.

  ```
  ibmcloud hpvs registration-create [--repository-name REPO-NAME] [--cr-username USER-NAME --cr-pwd-path FILE-PATH | --no-auth] [--allowed-env-keys ENV-KEYS | --no-env] [--image-key-id IMAGE-KEY-ID] [--image-key-public-path PUBLIC-KEY] [--registration-key-private-path PRIVATE-KEY-PATH] [--registration-key-public-path PUBLIC-KEY-PATH] [--gpg-passphrase-path PASS-PHRASE]
  ```
  {: pre}

  If you enter the command without any parameters:

  ```
  ibmcloud hpvs registration-create
  ```
  {: pre}
  You are prompted to enter all the parameters.

  If the container registry does not require authentication, set the `-no-auth` parameter to prevent prompting. If no environment parameters are required, set the `-no-env` parameter, for example:

  ```
  ibmcloud hpvs registration-create --no-env --no-auth
  ```
  {: pre}

  Where:
  <dl>
  <dt>`--repository-name REPO-NAME`</dt>
  <dd>Is the fully qualified name for the repository.</dd>
  <dt>`--cr-username USER-NAME`</dt>
  <dd>Is the username for the login on the container repository. It can be any string of 4 - 30 characters.</dd>
  <dt>`--cr-pwd-path FILE-PATH`</dt>
  <dd>Is the path for the file that contains the container repository password.</dd>
  <dt>`--no-auth`</dt>
  <dd>Is the parameter that must be set if the image does not require authorization to download. In this case,  you don't need to provide `cr-username` and `cr-pwd-path` parameters. If you do, these parameters are ignored. </dd>
  <dt>`--allowed-env-keys ENV-KEYS`</dt>
  <dd>Specifies the allowed environment variable keys as a comma-separated list. The keys must be strings of 1 - 64 characters.</dd>
  <dt>`--no-env`</dt>
  <dd>This parameter can be set if the image does not require any allowed environment variables. In this case,  you don't need to provide the `allowed-env-keys` parameter. If you do, it is ignored.</dd>
  <dt>`--image-key-id IMAGE-KEY-ID`</dt>
  <dd>The ID of the root key that was used to sign the image. It must contain 64 characters. If the image-key-id is not specified, the command first tries to determine the ID automatically by requesting the container registry notary service. Optional.</dd>
  <dt>`--image-key-public-path PUBLIC-KEY`</dt>
  <dd>The path for the file that contains the public part of the key that is used to sign the image. The public part of the key must be a minimum of 20 characters and base64 encoded. If the path is not specified, the command first tries to determine the public part of the key automatically by requesting the container registry notary service. Optional.</dd>
  <dt>`--registration-key-public-path PRIVATE-KEY-PATH`</dt>
  <dd>The path for the public key from the registration key pair.</dd>
  <dt>`--registration-key-private-path PUBLIC-KEY-PATH`</dt>
  <dd>The path for the private key from the registration key pair.</dd>
  <dt>`--gpg-passphrase-path PASS-PHRASE` </dt>
  <dd>The path for the `gpg` pass phrase used for the private part of the registration key. The passphrase must consist of at least 6 characters. To make sure that a new line is not appended, use `echo` with `-n` or `cat` with EOF.</dd>
  </dl>


## Adding Linux capabilities to the registration definition file
{: #byoi_regdef}
The registration definition file contains metadata about the OCI image you want to use for your Hyper Protect Virtual Server, such as the repository name and the credentials to pull the image. Because it can contain secret information, you must encrypt and sign the file before you use it to create a new Virtual Server. You can encrypt and sign the file on any system on which `gpg` is installed. To use a Hyper Protect Virtual Server, install the `gpg` package by running `apt update && apt install -y gpg`.

1. Create a file and copy this template for registration definition files into it:
   ```
   {
         "repository_name": "",
         "docker_username": "",
         "docker_password": "",
         "envs_whitelist": ["RUNQ_ROOTDISK", "RUNQ_RUNQENV", "RUNQ_SYSTEMD", "IMAGE_TAG", "REGION", "PHASE", "LPAR_NAME", "CPC", "RUNQ_CPU", "RUNQ_MEM", "POD"],
         "public_key_id": "",
         "public_key": "",
         "vendor_key": "",
         "cap_add": ["ALL"],
   }
   ```
   {: codeblock}  

   {:note}
   Optionally, you can add Linux capabilities for your {{site.data.keyword.hpvs}} instance. The list of Linux capabilities are available at [Capabilities](https://man7.org/linux/man-pages/man7/capabilities.7.html). All the capabilities listed are supported except "CAP_PERFMON", "CAP_BPF", and CAP_CHECKPOINT_RESTORE". While adding the capabilities, you must remove the prefix "CAP". For example, CAP_AUDIT_CONTROL must be specified as AUDIT_CONTROL. To enable all privileges, you can use "cap_add": ["ALL"], but as a good security practice, provide the least possible privileges for your virtual server instance.
1. Enter the value for the repository name in the `repository_name` field in the template. This parameter requires the full path to the OCI image without the tag, for example, `docker.io/library/ubuntu/` or `de.icr.io/hpvs/ubuntu`.
1. Enter your credentials to authenticate on the registry in the `docker_username` and `docker_password` fields. If you use {{site.data.keyword.cloud_notm}} Container Registry, your username is `iamapikey` and the password is your API key. If your image does not require authentication, leave the values for `docker_username` and `docker_password` empty.
1. Run `docker trust inspect <image>` and copy the Root key ID from the AdministrativeKeys that was used to sign the Docker Image on your build system.  Paste the ID into the `public_key_id` value field in the template, for example:
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
1. Open the Docker metadata file `~/.docker/trust/tuf/<image>/metadata/root.json` and locate the Key ID from the previous step. Copy the public key from that file and paste it in the `public_key` field. With that step, the content of the template is complete.
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
1. Run `gpg -a --batch --generate-key <batchfile>` to create a new keypair as the `isv_user` identity, which is used for signing the registration file in a later step:
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
1. Export the public part of the key by running the following command:
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
1. Copy the following public key, which is used for encrypting the registration file, into a file:
  ```
  -----BEGIN PGP PUBLIC KEY BLOCK-----
  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
  8ty0LuCdUvbKsEuietRUgjEzuZ8FrA2q
  =y8qG
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

  The signature is done with the private key you generated earlier and the file is encrypted by using the public key that you imported.


## Using your OCI Image to provision a Hyper Protect Virtual Server
{: #byoi_provision}
You must use the [CLI](https://cloud.ibm.com/docs/hpvs-cli-plugin) `ibmcloud hpvs instance-create` command to use your own OCI Image to provision a Hyper Protect Virtual Server, as described [here](https://test.cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-provision#provision-cli).

### Example
{: #byoi_example}

```
ibmcloud hpvs instance-create myServerName lite-s dal13 --rd-path "C:\MyRegistrationDefinitions\registration.json.asc" -i latest
```

## Configuring your server
{: #byoi_config}


The following topic describes how you can configure your server. Use this description only for configurations that are not considered credentials or personal data.

{:note}
In this description "environment variables" are not set as environment variables, instead they provided within `/.runqenv`.

Instance-specific configurations for applications that run in OCI containers are generally provided as environment variables when a container is created.
Either the application consumes the variables, or a script, that runs before the actual application,   configures the application.
Before you can set environment variables, the registration definition must allow them.
You can provide environment variables by way of the  `e` or `--environment` option (cannot contain “,”). The environment variables are written to `/.runqenv`.

You can allow environment variables by name to `envs_whitelist` array within the registration definition.

The following variables cannot be set: “RUNQ_ROOTDISK”, “RUNQ_RUNQENV”, “RUNQ_SYSTEMD”, “IMAGE_TAG”, “REGION”, “PHASE”, “LPAR_NAME”, “CPC”, “RUNQ_CPU”, “RUNQ_MEM”, “POD”.

Variable names can have up to 64 characters and can consist of numbers, chars, and underscore. Variable values can have up to 4096 characters and can consist of base64 character set characters (uppercase letters, lowercase letters, and +, /).

**An example with one environment variable:**
```
ibmcloud hpvs instance-create myServerName lite-s dal13 --rd-path “C:\MyRegistrationDefinitions\registration.json.asc ” -i latest -e name=value
```

**An Example with multiple environment variables:**
```
ibmcloud hpvs instance-create myServerName lite-s dal13 --rd-path “C:\MyRegistrationDefinitions\registration.json.asc ” -i latest -e name1=value1 -e name2=value2
```

The initial user has the corresponding environment variable set within the container.
