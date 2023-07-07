---

copyright:
  years: 2023
lastupdated: "2023-02-27"

subcollection: hp-virtual-servers

keywords: public keys, private keys, Red Hat simple signing, update virtual server instance, instance, virtual server

---


{{site.data.keyword.attribute-definition-list}}

# Switching from DCT to Red Hat simple signing and updating the virtual server
{: #switchdct_toicr}

The Notary v1 service that supports Docker Content Trust (DCT) and docker trust commands (including the alternative URLs) in {{site.data.keyword.cloud}} Container Registry is discontinued. Any existing ICR images that were signed by using DCT, must be re-signed by using Red Hat. For more information, see [Release notes for Container Registry](/docs/Registry?topic=Registry-registry_release_notes#registry-01nov2021).
{: note}


## Prerequisites
{: #pre_dctredhatsign}

- Not applicable if your image is DCT signed and present in DockerHub.
- Not applicable if the image is Red Hat simple signed and present in {{site.data.keyword.cloud}} Container Registry (ICR), and the virtual sever instance is already deployed with a Red Hat Simple Signed Image.
- Not applicable if your virtual server is already running and you do not plan to update the virtual server.
- Applicable only when the image is signed by using Docker Content Trust (DCT) and the image is present in ICR, and you plan to update the virtual server to the latest version. In this scenario, follow [these](#proc_migratedct_icr) instructions.
- You must install [Skopeo](https://github.com/containers/skopeo/blob/main/install.md).


## Procedure
{: #proc_migratedct_icr}

1. Run the following command to list all your {{site.data.keyword.hpvs}} instances:
   ```sh
   ibmcloud hpvs instances [--output json]
   ```
{: pre}

   `--output`
   :   Displays results in the requested format. The only valid value is `json`.

2. Run the following command to view details about the specific server instance:

   ```sh
   ibmcloud hpvs instance (NAME | CRN) [--output json]
   ```
   {: pre}


   `NAME`
   :   The name of your instance.

   Specify the `CRN` if your instance name is not unique.

   `CRN`
   :   The server's Cloud resource name (CRN). Specify the `CRN` if `NAME` is not unique. You can run the `ibmcloud hpvs instances` command to get the CRN.


   **Command options**

   `--output`
   :   Displays results in the requested format. The only valid value is `json`.


   **Example output**

   ```json
   Name                  hpvs-env-test
   CRN                   crn:v1:bluemix:public:hpvs:..............::
   Location              <location>
   Cloud tags
   Cloud state           active
   Server status         running
   Plan                  Free
   Public IP address     52.x.x.x
   Internal IP address   172.x.x.x
   Storage               50 GiB
   Memory                2048 MiB
   Processors            1 vCPUs
   Image type            self-provided
   Image OS              self-defined
   Image name            de.icr.io/hpvs_test/ubuntu:v2
   Environment           TEST_VAR=value
                         TEST_VAR2=value2
   Last operation        update succeeded
   ```
   {: screen}


3. Complete the following steps to sign the images by using Red Hat simple signing:
   1. Create a batch file and add the following content to it:
      ```sh
      Key-Type: RSA
      Key-Length: 4096
      Subkey-Type: RSA
      Subkey-Length: 4096
      Name-Real: latestnewkey
      Name-Email: latestnewkey@example.com
      Expire-Date: 472
      ```
      {: codeblock}

   2. Create the keys.
      ```sh
      gpg --pinentry-mode loopback --passphrase="" --generate-key --batch <batchfile>
      ```
      {: pre}

   3. Export the public key.
      ```sh
      gpg --armor --export <name> > <filename>.pub
      ```
      {: pre}

   4. Sign the image using `skopeo` and push the image to ICR.
      ```sh
      skopeo copy docker-daemon:us.icr.io/yournamespace/nginx1:latest docker://us.icr.io/yournamespace/nginx1:latest --sign-by E8C9E90........D9F3 --dest-creds=iamapikey:*****************

      Getting image source signatures
      Copying blob 226117031573 done
      .........
      Writing manifest to image destination
      Signing manifest using simple signing
      Storing signatures
      ```
      {: codeblock}

      **Example output**

      ```sh
      sudo skopeo copy docker-daemon:us.icr.io/yournamespace/nginx1:latest docker://us.icr.io/yournamespace/nginx1:latest --sign-by E8C9E9091D087114C7A978FF82CA7961A927D9F3 --dest-creds=iamapikey:*****************

      Getting image source signatures
      Copying blob 226117031573 done
      Copying blob 04ab349b7b3b done
      Copying blob 2c31eef17db8 done
      Copying blob 7b9055fc8058 done
      Copying blob 73993eeb8aa2 done
      Copying blob 6485bed63627 done
      Copying blob 1f9171ae3ae1 done
      Copying config bd909010fa done
      Writing manifest to image destination
      Signing manifest using simple signing
      Storing signatures
      ```
      {: screen}

4. Update to the latest `hpvs plugin` by running the following command:
   ```sh
   ibmcloud plugin install hpvs
   ```
   {: pre}

   **Example output**

   ```sh
   ibmcloud plugin install hpvs
   Looking up 'hpvs' from repository 'IBM Cloud'...
   Plug-in 'hpvs 1.4.24' found in repository 'IBM Cloud'
   Plug-in 'hpvs 1.4.21' was already installed. Do you want to update it with 'hpvs 1.4.24' or not? [y/N] > y
   Attempting to download the binary file...
    13.88 MiB / 13.88 MiB [========================================================================================================================================================================] 100.00% 2s
   14553504 bytes downloaded
   Installing binary...
   OK
   Plug-in 'hpvs 1.4.24' was successfully installed into bluemix/plugins/hpvs. Use 'ibmcloud plugin show hpvs' to show its details.
   ```
   {: screen}

5. Run the following command to create a registration definition file
   ```sh
   ibmcloud hpvs registration-create [--repository-name REPO-NAME] [--cr-username USER-NAME --cr-pwd-path FILE-PATH | --no-auth] [--allowed-env-keys ENV-KEYS | --no-env] [--image-key-id IMAGE-KEY-ID] [--fingerprint FINGERPRINT] [--image-key-public-path PUBLIC-KEY] [--registration-key-private-path PRIVATE-KEY-PATH] [--registration-key-public-path PUBLIC-KEY-PATH] [--gpg-passphrase-path PASS-PHRASE] [--cap-add  CAPABILITIES] [--isv-secrets ISV-SECRETS | --no-isv-secrets]
   ```
   {: pre}

   The output of this command is a `registration.json.asc` file that is used for updating the virtual server. Ensure that you move the existing `registration.json.asc` file to another location before you run this command.

   If you enter the command without any parameters, you are prompted to enter all the parameters.

   If image is in ICR, the fingerprint of the gpg key that was used to sign the image must be provided when you are prompted for the "Fingerprint", and path to the public key with which it is signed needs to be provided when you are prompted for the "Path to the file containing the image public key".
   {: note}

   For the `--registration-key-private-path` and `--registration-key-public-path` parameters, ensure that you use the same private and public key that you had used when you created the virtual instance the first time. You must save these keys in a safe and secure location for future use. If you do not use the same set of private and public key, the command execution fails
   {: note}

   If the container registry does not require authentication, set the `-no-auth` parameter to prevent prompting. If no environment parameters are required, set the `-no-env` parameter, for example:

   ```sh
   ibmcloud hpvs registration-create --no-env --no-auth
   ```
   {: pre}

   **Command options**
   `--repository-name REPO-NAME`
   :   Is the fully qualified name for the repository.

   `--cr-username USER-NAME`
   :   Is the username for the login on the container repository. It can be any string of 4 - 30 characters.

   `--cr-pwd-path FILE-PATH`
   :   Is the path to the file that contains the container repository password.

   To ensure there are no trailing spaces in the file path, you can specify it as `vi -b file_name, :set noeol, :wq`.
   {: note}

   `--no-auth`
   :   Is the parameter that must be set if the image does not require authorization to download. In this case, you don't need to provide `cr-username` and `cr-pwd-path` parameters. If you do, these parameters are ignored.

   `--allowed-env-keys ENV-KEYS`
   :   Specifies the allowed environment variable keys as a comma-separated list. The keys must be strings of 1 - 64 characters.

   `--no-env`
   :   This parameter can be set if the image does not require any allowed environment variables. In this case, you do not need to provide the `allowed-env-keys` parameter. If you do, it is ignored.

   `--image-key-id IMAGE-KEY-ID`
   :   The ID of the root key that was used to sign the image. It must contain 64 characters long. If the image-key-id is not specified, the command first tries to determine the ID automatically by requesting the container registry notary service. You are prompted for this parameter for Docker Hub images.

   `--fingerprint FINGERPRINT`
   :   If the image is in ICR, the fingerprint of the gpg key that is used to sign the image must be provided. You can get the fingerprint by using the `gpg --list-keys name_of_the_key` command. For example, when you run the `gpg --list-keys latestnewkey` command, the following snippet is an example of the output:
    ```buildoutcfg
    pub   rsa4096 2023-01-12 [SCEA] [expires: 2024-04-28]
          E8C9E...........61A927D9F3
    uid           [ultimate] latestnewkey
    <latestnewkey@example.com>
    sub   rsa4096 2023-01-1--fingerprint FINGERPRINT2 [SEA] [expires: 2024-04-28]
    ```
    {: screen}

    In this example, "E8C9E...........61A927D9F3" is the  fingerprint.

   `--image-key-public-path PUBLIC-KEY`
   :   The path for the file that contains the public part of the key that was used to sign the image. The public part of the key must be a minimum of 20 characters and base64 encoded. If the image is present in ICR, path to the gpg public key that is used to sign the image and push to ICR should be mandatorily provided.

   `--registration-key-public-path PRIVATE-KEY-PATH`
   :   The path for the public key from the registration key pair. Ensure that you use the same key that you had used when you created the  virtual instance the first time. You must save these keys in a safe and secure location for future use.

   `--registration-key-private-path PUBLIC-KEY-PATH`
   :   The path for the private key from the registration key pair. Ensure that you use the same key that you had used when you created the  virtual instance the first time. You must save these keys in a safe and secure location for future use.

   `--gpg-passphrase-path PASS-PHRASE`
   :   The path to a file that contains the `gpg` passphrase used for the private part of the registration key. The passphrase must consist of at least 6 characters. To make sure that a new line is not appended, use `echo` with `-n` or `cat` with EOF.

   `--cap-add CAPABILITIES`
   :   The Linux capabilities to be enabled are specified as a comma separated list.

   `--isv-secrets ISV-SECRETS`
   :   The Linux secrets to be used in BYOI. The secrets are added in the `/isv_secrets/secrets.json` file, within the container.

   The ISV secrets is a key value pair that is separated by a colon, and you can specify a list secrets that are separated by a comma. Avoid adding spaces after the comma when you are specifying multiple secrets. If the size of the secret is large, it is recommended that you specify it by using the `--isv-secrets` command parameter. To update ISV secrets, you must create a new registration definition file with the updated ISV secrets and use the same registration definition file when running the `hpvs instance-update` command to update the instance.
   {: note}

   `--no-isv-secrets`
   :   If the image does not require any isv secrets, this parameter can be set. You need not provide the isv-secrets parameter, and if you do, it will be ignored.

   If you do not use the same private and public keys (`--registration-key-public-path PRIVATE-KEY-PATH` and `--registration-key-private-path PUBLIC-KEY-PATH`) that you had used when you first created the virtual server instance, then command execution fails.

   **Example output**

   ```sh
   ibmcloud hpvs registration-create
   Repository name> us.icr.io/yournamespace/nginx1
   Container registry API key>
   Secrets to be passed into BYOI registration file>
   Allowed environment variables as comma separated list>
   Fingerprint> E8C9E...........61A927D9F3
   Path to the file containing the image public key> new.pub
   Path to the file containing public part of the registration key> byoifeb21.public
   Allowed Linux capabilities to be enabled as comma separated list> ALL
   Path to the file containing the private part of the registration key> byoifeb21.private
   Gpg pass phrase for the private part of the registration key>
   OK
   The registration file was successfully created in the current working directory: registration.json.asc
   Complete command executed:
    ibmcloud hpvs registration-create --repository-name=us.icr.io/yournamespace/nginx1 --cr-username=iamapikey --cr-pwd-path=<PATH> --no-env --fingerprint=E8C9E...........61A927D9F3 --image-key-public-path=new.pub --registration-key-public-path=byoifeb21.public --cap-add=ALL --registration-key-private-path=byoifeb21.private --gpg-passphrase-path=<PATH>
   ```
   {: screen}

6. Run the following command to update the virtual server instance:
   ```sh
   ibmcloud hpvs instance-update (NAME | CRN) [--hostname HOST-NAME] [(--rd REGISTRATION-DEFINITION | --rd-path REGISTRATION-DEFINITION-PATH)] [-i IMAGE-TAG] [-e ENV-CONFIG1 -e ENV-CONFIG2 ...] [--force]
   ```
   {: pre}


   `NAME`
   :   The name of your your instance.

   Specify the `CRN` if your instance name is not unique.

   `CRN`
   :   The server's Cloud resource name (CRN). Specify the `CRN` if `NAME` is not unique. You can run the `ibmcloud hpvs instances` command to get the CRN.

   **Command options**

   `--hostname HOST-NAME`
   :   The hostname that will be set within the {{site.data.keyword.hpvs}} container by using this parameter value.

   `--rd REGISTRATION-DEFINITION`
   :   The encrypted and signed registration definition that is used for [Bring your own server image (BYOI)](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi). `--rd` or `--rd-path` is optional when you use a self-provided image.

   `--rd-path REGISTRATION-DEFINITION-PATH`
   :   File path to the file that contains the encrypted and signed registration definition that is used for BYOI. `--rd` or `--rd-path` is optional when you use a self-provided image.

   `-i IMAGE-TAG`
   :   The image tag for the BYOI server image. Required if you're using your own image.

   `-e ENV-CONFIG`
   :   Specify environment variables when you use a self-provided image. They need to be specified in your registration definition first. You can set one or more environment variables as key value pairs by using the `-e` flag, for example, `-ibmcloud hpvs instance-update CRN -i latest -e k1=v1 -e k2='v2 v3'`. Environment variable `names` can have a maximum length of 64 characters and can be numbers, chars, and underscore. Environment variable `values` can have a maximum length of 4096.

   `--force`
   :   Forces the update of the {{site.data.keyword.hpvs}} instance without prompting for confirmation.


   **Example output**

   ```sh
   ibmcloud hpvs instance-update hpvs-env-test -i latest --rd-path "registration.json.asc"

   This will wipe all the data from your virtual boot disk of the virtual server and replace it with a new image. Make sure that you have moved all required data to the /data/ disk. The Ubuntu virtual server SSH server is configured to read the authorized keys for the root user from the /data/.ssh/authorized_keys file after the update. Verify that this file contains a valid configuration so that you don't lose access to your instance.

   Are you sure you want to update the service instance? [y/N]> y
   OK
   Update request for service instance 'hpvs-env-test (crn:v1:bluemix:public:hpvs:..............::)' was accepted.
   ```
   {: screen}
