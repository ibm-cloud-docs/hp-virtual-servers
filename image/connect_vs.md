---

copyright:
  years: 2019
lastupdated: "2019-08-28"

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Logging-in to a virtual server
{: #connect_vs}

For a successfully provisioned instance, you can log in to this server by using your preferred method.
{:shortdesc}

{:note}
Due to configuration processes, you need to wait up to 30 minutes until you can connect to a new virtual server (HP-VS instance) for the first time.

## Logging-in from a Unix-like operating system

The syntax of log-in commands in Unix-like operating systems is nearly the same in all such operating systems.
Examples for Unix-like operating systems are:
* Linux
* Windows Subsystem for Linux (WSL)
* Windows using Git Bash
* macOS

For example, in Windows, you can open a Git Bash session and log-in as *root* user with the following additional information:
* Specify the public IP address of the created instance (see \ref{f_retrieve_info_1}). You can find it on the {{site.data.keyword.hpvs}} dashboard as described in [Retrieving virtual server information](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs). In the shown command example, `198.51.100.21` is used as IP address.
* Specify the file that contains your private key with the `-i` parameter. In the command example, `/c/cloud_keys/id_rsa` is used as file name and location of the private key.  

`$ ssh root@<public_ip_address> -i <path_to_priv_key_file>`

```
$ ssh root@198.51.100.21 -i /c/cloud_keys/id_rsa
```
{: codeblock}


If you log in for the first time, the new server is not yet known to SSH, and therefore, you receive the following message:  

`
The authenticity of host '198.51.100.21 (198.51.100.21)' can't be established.
ECDSA key fingerprint is SHA256:dVaFv4slDyiYZcUV5F6MKqhxIo4SuucWkXVQoc4MTIk.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
`

Answer `yes` to allow SSH to add your server to the list of known hosts:

`Warning: Permanently added '169.63.213.157' (ECDSA) to the list of known hosts.`
`Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-47-generic s390x)`


## Logging-in from Windows by using **PuTTY Configuration**
{: #connect_vs_with_putty}


A convenient method to open terminal sessions is to use the **PuTTY Configuration** utility.
You can ease the logon and authentication to your server with your private SSH key.

1. Define the virtual server in the **Session** category of **PuTTY Configuration**.
   * Enter the server's host name or public IP address as shown.
   * Enter a name into the **Saved Sessions** field. In the shown example, this session name is the name as proposed during provisioning.
   * Click **Save**.

   ![Defining a new virtual server instance](image/hpvs_define.jpg "Defining a new virtual server instance")

   *Figure 1. Defining a new virtual server instance*
2. In the **SSH -> Auth** category, specify the file that contains the private SSH key.  

   ![Providing the private SSH key for authentication](image/hpvs_ssh_auth.jpg "Providing the private SSH key for authentication")

   *Figure 2. Providing the private SSH key for authentication*

   If you generated an SSH key pair other than using **PuTTY**, you can use **Conversions -> Import key** of the **PuTTY Key Generator** to convert the private key into the **PuTTY** format of the same length.
3. Define the **root** default user name under **Connection -> Data**.

   <img src="image/hpvs_root.jpg" alt="Defining the auto-login user name" width="450" style="width: 450px; border-style: none"/>

   *Figure 3. Defining the auto-login user name*
4. Go back to the session configuration shown in Figure 1 and click **Save** again. Otherwise you loose your changes.

From now on, you can open and log in to this server as **root** user without the need to specify your private SSH key.
