---

copyright:
  years: 2021
lastupdated: "2021-08-05"

subcollection: hp-virtual-servers

content-type: tutorial
services: hp-virtual-servers
account-plan: free
completion-time: 1h

keywords: secure networking, secure network, virtual server instance, instance, virtual server, VPN, secure CI/CD, confidential computing, confidential computing build, confidential computing CI/CD
---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}


# Tutorial: Setting up the secure network
{: #tutorial_network}
{: toc-content-type="tutorial"}
{: toc-services="hp-virtual-servers"}
{: toc-completion-time="1h"}

This tutorial describes how you can set up the secure network which provides an end to end encrypted network communication for {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} services. This tutorial is intended for IBM Cloud Hyper Protect Virtual Servers customers. Contact your IBM representative for access to the GitHub repository.
{: shortdesc}

## Contents
- [Basic concepts](#basicconcept_sn)
- [Before you begin](#prerequisites_sn)
- [Configuring your desktop](#download_cli)
- [Task flow](#taskflow_sn)
- [Command reference](#ref_sn)

## Basic concepts
{: #basicconcept_sn}

Some of the key features of the secure network are listed here:
- An end-to-end encrypted network.
- Controlled by the user rather than the cloud provider. The secure network configuration for a virtual server is managed by the user who creates the Hyper Protect Virtual Server instance, and this configuration cannot be modified by the IBM Cloud IAM user, nor by the IBM Cloud operator.


A user with administrator access to the Hyper Protect Virtual Server must set up the entire secure network and can configure multiple users to this role, who have access to the capabilities to manage the secure network and deploy solutions on the secure network. Be aware that currently the secure network does not provide audit logs for commands that are sent by different users. Therefore, do not enable multiple users to access the same virtual server if it's not required.
{: note}


![Secure network architecture diagram](image/secure_network_arch.png "Secure network architecture")
*Figure 1. The Secure network architecture*

1. The secure network management agent that runs on the {{site.data.keyword.hpvs}} receives commands from the secure network management agent CLI and updates the Virtual Private Network (VPN)  configuration on the {{site.data.keyword.hpvs}}, as requested.
2. The secure network management agent CLI receives the user command inputs and updates the VPN configuration of the {{site.data.keyword.hpvs}} by sending  requests to the secure network management agent that is running on the {{site.data.keyword.hpvs}}.
3. The secure network opens two endpoints on the virtual server network interface:    
   - In the data plane: A WireGuard endpoint (UDP port) that is used by the WireGuard driver.
   - In the control plane: A management agent endpoint (TCP port) that is used by the management agent.     
4. The secure network stores configuration data in files. The data volume should be mounted on the virtual server local persistent storage for the  secure network configuration data. All network configuration files are mounted at `/data/hpagent_store`.
5. The {{site.data.keyword.cloud}} Identity and Access Management (IAM) is used for user authentication. For more information, see [IBM Cloud  Identity and Access Management](https://cloud.ibm.com/docs/account?topic=account-iamoverview).


## Before you begin
{: #prerequisites_sn}

To complete this tutorial, you need to meet the following prerequisites:
- Administrator access to the Hyper Protect Virtual Server.
- Create an {{site.data.keyword.cloud_notm}} account.
- Two {{site.data.keyword.hpvs}} instances.


## Configuring your desktop
{: #download_cli}

Complete the following steps:
1. To install the secure network management CLI, download the files `dap-public.pem`, `encryptedNetworkCli-1.0.0.tar.gz`, and  `encryptedNetworkCli-1.0.0.tar.gz.sig` from this [GitHub repository](https://github.com/ibm-hyper-protect/secure-network/releases/tag/v1.0.0). After you save the files to any location on your desktop, complete the following steps:
   - To verify secure network management agent's signature, run the following command.
     ```sh
     openssl dgst -sha256 -verify dap-public.pem -signature encryptedNetworkCli-1.0.0.tar.gz.sig encryptedNetworkCli-1.0.0.tar.gz
     ```
     {: pre}

     The following snippet shows an example output.
     ```sh
     encryptedNetworkCli-1.0.0.tar.gz Verified OK
     ```
     {: screen}

   - To extract the compressed file, run the following command.
     ```sh
     tar -zxvf encryptedNetworkCli-1.0.0.tar.gz
     ```
     {: pre}

     After you successfully extract the compressed file, the folder 'encryptedNetworkCli' contains a list of the CLI files that are provided for different operating systems or architecture. Choose the one suits your environment, and rename it to `hpnet`. For example, to rename the file on macOS, you can run the following command.
     ```sh
     mv ./hpnet-cli.mac.amd64.1.0.0 ./hpnet
     ```
     {: pre}

2. To obtain an API key for your IBM cloud account, follow these [instructions](https://cloud.ibm.com/docs/account?topic=account-userapikey#create_user_key).

   Export this key which you will use in the following sections by running the following command:
   ```sh
   export APIKEY=<your_apikey>
   ```
   {: pre}

3. For more details about each command that is supported by the secure network management CLI, see [Command reference](#ref_sn).


## Task flow
{: #taskflow_sn}

To complete this solution, you walk through the following steps:

1. [Configuring the virtual servers](#config_vs)
2. [Admin key administration](#manage_adminkey)
3. [VPN administration](#vpn_admin)
4. [End to end encryption via the VPN tunnel](#vpn_tunnel)

## Step 1: Configuring the virtual servers
{: #config_vs}

The {{site.data.keyword.hpvs}} instances are considered as 'HPVS_A', and 'HPVS_B', in this tutorial. For each of these two instances, complete the following steps:
1. To install CA certificates, run the following command:
   ```sh
   apt-get update && apt-get install -y ca-certificates
   ```
   {: pre}

2. Create the `authorized_user` file and save it at `/data/hpagent_store/authorized_user`, with a list of {{site.data.keyword.cloud_notm}} user IDs. The secure network management agent uses this list to approve a request and rejects any request that is not in this list.
   To create the `authorized_user` file, run the following commands:    
   ```sh
   mkdir -p /data/hpagent_store

   cat <<EOF > /data/hpagent_store/authorized_user
   <email_of _authorized_user>
   EOF
   ```
   {: codeblock}

3. You must configure iptables rules for the Hyper Protect Virtual Server to open ports for the agent and WireGuard, otherwise the secure network management agent CLI will not be able to connect to the secure network management agent, or other nodes will not be able to connect the virtual server via the WireGuard VPN IP. The default port for the secure network management agent is 5566, which cannot be changed. The port value for WireGuard should be a value between 1024 - 65535. This tutorial uses a value of 6666 for both HPVS_A and HPVS_B.
   To configure the iptables rules, run the following command:      
   ```sh
   WGPORT=6666
   iptables -A INPUT -p tcp --dport 5566 -j ACCEPT
   iptables -A INPUT -p tcp --dport ${WGPORT} -j ACCEPT
   ```
   {: pre}

   If the secure network management agent is deployed in a Hyper Protect Virtual Server instance which is in the staging environment (you should not run this command in the production environment), then you must export the following variable:
   ```sh
   export isProduction=false
   ```
   {: pre}

4. To install the secure network management agent, download the files `dap-public.pem`, `encryptednetwork-1.0.0.tar.gz`, and  `encryptednetwork-1.0.0.tar.gz.sig` from this [GitHub repository](https://github.com/ibm-hyper-protect/secure-network/releases/tag/v1.0.0). After you save the files to `/root`, or `/data`, and complete the following steps:
   - To verify secure network management agent's signature, run the following command.
     ```sh
     openssl dgst -sha256 -verify dap-public.pem -signature encryptednetwork-1.0.0.tar.gz.sig encryptednetwork-1.0.0.tar.gz
     ```
     {: pre}

     The following snippet shows an example output.
     ```sh
     encryptednetwork-1.0.0.tar.gz Verified OK
     ```
     {: screen}

   - To extract the compressed file, run the following command.
     ```sh
     tar -zxvf encryptednetwork-1.0.0.tar.gz
     ```
     {: pre}

     After you successfully extract the compressed file, the folder 'encryptednetwork' contains a file `encryptednetwork`
   - To install and launch the secure network management agent, run the following command:
     ```sh
     nohup ./encryptednetwork > log.txt 2>&1 &
     ```
     {: pre}

     The secure network management agent is now running.
     The following network configuration files are mounted at `/data/hpagent_store`:
     ```sh
     /data/hpagent_store/authorized_user
     /data/hpagent_store/public.pem
     /data/hpagent_store/private.key
     /data/hpagent_store/agent_config
     /data/hpagent_store/wgconfigs
     ```
     {: screen}

     The `agent_config`, and `wgconfigs` files are automatically generated by the secure network management agent later.
     You must configure only the `/data/hpagent_store/authorized_user` file. All the other files are created and maintained by the secure network management agent. You must ensure that the contents of the `/data/hpagent_store/agent_config` file are not modified and is protected from inadvertent exposure because that can cause potential attacks on the system. To keep the data at this location safe, you should follow the guidance on [backup and restore](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-disaster_hpvs).  

After you complete these steps, the secure network management agent starts listening on the specified network port and is ready to accept requests from the secure network management CLI. These tasks can be done manually after connecting to the {{site.data.keyword.hpvs}} instance by using SSH, or when creating a container image in the case of BYOI.

To prevent any malicious access, consider the following recommendations to control access to the Hyper Protect Virtual Server:
- The secure network management agent binary and configuration files are highly sensitive and should never be exposed or altered. It is your responsibility to control access to the Hyper Protect Virtual Server.
- Disable SSH access for your production environment when you have highly sensitive workloads. When you need to grant SSH access to the Hyper Protect Virtual Server to multiple users, you must control the permissions of SSH users, and configure the Hyper Protect Virtual Server file system permission to allow only designated users to access the secure network management agent binary and all configuration files, and prevent any other users from having any read, write, or execute on these files.
- It is your responsibility to keep the SSH private key secure, if SSH is enabled for the Hyper Protect Virtual Server.  

To prevent malicious access to the desktop that you used to set up and configure the secure network, consider the following recommendations:
- You must download and install the secure network management CLI only from this [Github repository](placeholder for link), with the correct signature provided by IBM. Do not install any programs from any other address that does not have the correct signature from IBM, as it might be malicious and can cause potential attacks on the virtual server.
- It is your responsibility to keep your desktop and the secure network management CLI binary secure.
- It is your responsibility to encrypt the admin private key and keep it secure.


## Step 2: Admin key administration
{: #manage_adminkey}

The secure network management CLI stores the newly generated key pair in the local file system, and you must ensure that the private key is kept secure. The secure network management CLI uses the private key to sign the request to upload the public key to the secure network management agent over the secure connection. The user API key is sent along with the public key. The secure network management agent uses the user API key to perform integrated authentication with IBM Cloud IAM and extract the user ID. The uploaded admin public key is associated with the user ID.

After this is complete, the secure network management agent is ready to accept requests from the secure network management CLI to perform secure network administration including admin key management and VPN management. The administration requests must include the user's user API key for integrated authentication with {{site.data.keyword.cloud_notm}} IAM, and signed with the user's admin private key (except reading requests which do not make any changes on the secure network).

See [Parameter specification](#param_limit) for details about some specifications that apply to some of the parameters of the secure network management CLI commands.

### Setting up a new admin key pair

1. To set the `adminkey` by using an auto-generated admin keypair on {{site.data.keyword.hpvs}} instance 'HPVS_A', complete the following steps:
   - To export the agent URL which you will use in the following sections, run the following command.
     ```sh
     export AGENT_URL_A=<HPVS_A_IP>:5566
     ```
     {: pre}

     where `HPVS_A_IP` is the IP address of 'HPVS_A'.
   - To set the `adminkey`, run the following command.
     ```sh
     ./hpnet adminkey set --apikey ${APIKEY} --url ${AGENT_URL_A}
     ```
     {: pre}

     The following snippet shows an example output when the command has completed successfully.
     ```sh
     +------------+--------------------------------------------------------------------+
     | KEYTYPE    | FILE                                                               |
     +------------+--------------------------------------------------------------------+
     | PrivateKey | /Users/sheryl/.hpnet-cfg/b1bd7b39-6496-cd75-4bd1-b29c2b775a3a.priv |
     | PublicKey  | /Users/sheryl/.hpnet-cfg/b1bd7b39-6496-cd75-4bd1-b29c2b775a3a.pub  |
     +------------+--------------------------------------------------------------------+

     An automatically generated keypair is stored locally and configured to node remotely, please keep the keypair carefully!!!

     command completed successfully.
     ```
     {: screen}

   - To export the private key which you will use in the following sections, run the following command.
     ```sh
     export ADMIN_PRIV_KEY_A=<PrivateKey_Path_for_HPVS_A>
     ```
     {: pre}

     where `PrivateKey_Path_for_HPVS_A` is the path to the file for the private key, which is displayed in the result of the `hpnet adminkey set` command.

2. To set the `adminkey` by using an auto-generated admin keypair on {{site.data.keyword.hpvs}} instance 'HPVS_B', complete the following steps:
   - To export the agent URL which you will use in the following sections, run the following command.
     ```sh
     export AGENT_URL_B=<HPVS_B_IP>:5566
     ```
     {: pre}

     where `HPVS_B_IP` is the IP address of 'HPVS_B'.
   - To set the `adminkey`, run the following command.
     ```sh
     ./hpnet adminkey set --apikey ${APIKEY} --url ${AGENT_URL_B}
     ```
     {: pre}

     The following snippet shows an example output when the command has completed successfully.
     ```sh
     +------------+--------------------------------------------------------------------+
     | KEYTYPE    | FILE                                                               |
     +------------+--------------------------------------------------------------------+
     | PrivateKey | /Users/sheryl/.hpnet-cfg/b1bd7b39-6496-cd75-4bd1-b29c2b775a3a.priv |
     | PublicKey  | /Users/sheryl/.hpnet-cfg/b1bd7b39-6496-cd75-4bd1-b29c2b775a3a.pub  |
     +------------+--------------------------------------------------------------------+

     An automatically generated keypair is stored locally and configured to node remotely, please keep the keypair carefully!!!

     command completed successfully.
     ```
     {: screen}

   - To export the private key which you will use in the following sections, run the following command.
     ```sh
     export ADMIN_PRIV_KEY_B=<PrivateKey_Path_for_HPVS_B>
     ```
     {: pre}

     where `PrivateKey_Path_for_HPVS_B` is the path to the file for the private key, which is displayed in the result of the `hpnet adminkey set` command.


## Step 3: VPN administration
{: #vpn_admin}

The VPN administration tasks must be performed by a user who has a valid {{site.data.keyword.cloud_notm}} user account (by providing a valid user API key) and a valid secure network admin key pair (by using [these](#manage_adminkey) procedures).

The VPN administration involves the following tasks:
- [Setting up a VPN node](#setup_vpnnode)
- [Setting up VPN trusted peers](#set_vpnpeer)

See [Parameter specification](#param_limit) for details about some specifications that apply to some of the parameters of the secure network management CLI commands.

### Setting up a VPN node
{: #setup_vpnnode}

Use the secure network management CLI to send a request to the secure network management agent to configure a new VPN node on the virtual server. The VPN key pair is generated by the secure network management agent automatically on the virtual server. You must specify other VPN node configuration parameters such as VPN IP, and endpoint, from the secure network management CLI.

Now a new VPN virtual network interface is created on the virtual server and configured with the parameters provided by the administrator, from the secure network management CLI.

1. To set up the VPN Node for 'HPVS_A', complete the following steps:
   - Export the following values which you will use in the following sections:
     ```sh
     export VPN_IP_A=10.10.10.1
     export WGPORT_A=6666
     ```
     {: codeblock}

     The WireGuard port value is set when you configure the iptables rules for the Hyper Protect Virtual Server. This tutorial uses a value of 6666.
   - To set up the VPN Node, run the following command:
     ```sh
     ./hpnet node set --apikey ${APIKEY} --url ${AGENT_URL_A} --admin-priv-key ${ADMIN_PRIV_KEY_A}  --vpnip ${VPN_IP_A} --wgport ${WGPORT_A}  --generate-keypair
     ```
     {: pre}

     The following snippet shows an example output when the command has completed successfully.
     ```sh
     +-----------------+----------------------------------------------+
     | NODE-PROPERTIES | VALUES                                       |
     +-----------------+----------------------------------------------+
     | VPNPublicKey    | ryKzTUEHcOy6b2CGSbK8zOpqTl/eTlpQZgIxH7R3twY= |
     | VPNIp           | 10.10.22.1/24                                |
     | WGPort          | 6666                                         |
     +-----------------+----------------------------------------------+

     command completed successfully.
     ```
     {: screen}

2. To set up the VPN Node for 'HPVS_B', complete the following steps:
   - Export the following values which you will use in the following sections:
     ```sh
     export VPN_IP_B=10.10.10.2
     export WGPORT_B=6666
     ```
     {: codeblock}

     The WireGuard port value is set when you configure the iptables rules for the Hyper Protect Virtual Server. This tutorial uses a value of 6666.
   - To set up the VPN Node, run the following command:
     ```sh
     ./hpnet node set --apikey ${APIKEY} --url ${AGENT_URL_B} --admin-priv-key ${ADMIN_PRIV_KEY_B}  --vpnip ${VPN_IP_B} --wgport ${WGPORT_B}  --generate-keypair
     ```
     {: pre}

     The following snippet shows an example output when the command has completed successfully.
     ```sh
     +-----------------+----------------------------------------------+
     | NODE-PROPERTIES | VALUES                                       |
     +-----------------+----------------------------------------------+
     | VPNPublicKey    | ryKzTUEHcOy6b2CGSbK8zOpqTl/eTlpQZgIxH7R3twY= |
     | VPNIp           | 10.10.22.2/24                                |
     | WGPort          | 6666                                         |
     +-----------------+----------------------------------------------+

     command completed successfully.
     ```
     {: screen}


### Setting up VPN trusted peers
{: #set_vpnpeer}

1. To set 'HPVS_B' as the VPN trusted peer for 'HPVS_A', complete the following steps:
   - Export the following values which you will use in the following sections:
     ```sh
     export PEER_VPN_IP_A=10.10.10.2
     export PEER_ENDPOINT_A=<HPVS_B_IP>:${WGPORT_B}
     export PEER_VPN_PUBLIC_KEY_A=<VPNPublicKey_of_VPN_node_for_HPVS_B>
     ```
     {: codeblock}

     where `peer-endpoint` should be `<HPVS_B_IP>:wgport`, and `peer-vpn-pub-key` is `VPNPublicKey` of the trusted peer, which is displayed in result of the `hpnet node set` command for HPVS_B.
   - To set 'HPVS_B' as the VPN trusted peer for 'HPVS_A', run the following command:
     ```sh
     ./hpnet peer add --apikey ${APIKEY} --url ${AGENT_URL_A} --admin-priv-key ${ADMIN_PRIV_KEY_A} --peer-vpn-ip ${PEER_VPN_IP_A}  --peer-endpoint ${PEER_ENDPOINT_A} --peer-vpn-pub-key ${PEER_VPN_PUBLIC_KEY_A}
     ```
     {: pre}

     The following snippet shows an example output when the command has completed successfully.
     ```sh
     command completed successfully.
     ```
     {: screen}

2. To set 'HPVS_A' as the VPN trusted peer for 'HPVS_B', complete the following steps:
   - Export the following values which you will use in the following sections:
     ```sh
     export PEER_VPN_IP_B=10.10.10.1
     export PEER_ENDPOINT_B=<HPVS_A_IP>:${WGPORT_A}
     export PEER_VPN_PUBLIC_KEY_B=<VPNPublicKey_of_VPN_node_for_HPVS_A>
     ```
     {: codeblock}

     where `peer-endpoint` should be `<HPVS_A_IP>:wgport`, and `peer-vpn-pub-key` is `VPNPublicKey` of the trusted peer, which is displayed in result of the `hpnet node set` command for HPVS_A.
   - To set 'HPVS_A' as the VPN trusted peer for 'HPVS_B', run the following command:
     ```sh
     ./hpnet peer add --apikey ${APIKEY} --url ${AGENT_URL_B} --admin-priv-key ${ADMIN_PRIV_KEY_B} --peer-vpn-ip ${PEER_VPN_IP_B}  --peer-endpoint ${PEER_ENDPOINT_B} --peer-vpn-pub-key ${PEER_VPN_PUBLIC_KEY_B}
     ```
     {: pre}

     The following snippet shows an example output when the command has completed successfully.
     ```sh
     command completed successfully.
     ```
     {: screen}

The configuration of secure networking now complete.  


Be aware that after a VPN node is added as a trusted peer, it can connect to your applications running on the virtual server via the VPN IP, therefore you must ensure that the added VPN node can be trusted and the information about the node is correct. If the added node is malicious, your application might be exposed to potential attacks.
{: note}


## End to end encryption via the VPN tunnel
{: #vpn_tunnel}

After setting up the admin key, the VPN node, and the VPN trusted peers, complete the following steps to encrypt the connection between the two Hyper Protect Virtual Server instances.

1. Start a service on 'HPVS_A' by binding to the VPN IP, which is ${VPN_IP_A}, as set earlier in this tutorial.
2. Have a client on 'HPVS_B' connect to the service running on 'HPVS_A' by using the VPN IP, which is ${VPN_IP_A}, as set earlier in this tutorial.
3. Now the connection between the client on HPVS_B and the service on HPVS_A are end to end encrypted via the VPN tunnel.


## Command reference
{: #ref_sn}


Ensure that you have exported the appropriate parameters before you run these commands.
{: note}

- To view the details of the commands and their parameters, type the command you want view with the `--help` option, for example the following command shows you details about the `hpnet peer add` command:
   ```sh
   ./hpnet peer add --help
   ```
   {: pre}

   The following snippet shows the output.
   ```sh
   to add a peer on a node
   Usage:
   hpnet peer add [flags]

   Flags:
         --admin-priv-key string     The the private key used to sign the request
         --apikey string             The APIKEY of the user
     -h, --help                      help for add
         --peer-endpoint string      The <peerip:wgport> for wireguard link
         --peer-vpn-ip string        The ipaddress (a.b.c.d) or CIDR format ipaddress (a.b.c.d/n) configured on the peer is used for wireguard link
         --peer-vpn-pub-key string   The base64 format of ed25519 public key which is used for wireguard link
         --url string                The url of the server
   ```
   {: screen}


- The `adminkey get` command
   ```sh
   ./hpnet adminkey get --apikey ${APIKEY} --url ${AGENT_URL}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   +------------+----------------------------------------------+
   | PROPERTIES | VALUES                                       |
   +------------+----------------------------------------------+
   | PublicKey  | HnPEmF1YP8S820M5iAfNvIdjT+y9X15iOkfl4+nQVYw= |
   +------------+----------------------------------------------+

   command completed successfully.
   ```
   {: screen}

- The `adminkey delete` command
   ```sh
   ./hpnet adminkey delete --apikey ${APIKEY} --url ${AGENT_URL} --admin-priv-key ${ADMIN_PRIV_KEY}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   command completed successfully.
   ```
   {: screen}

- The `vpn node get` command
   ```sh
   ./hpnet node get --apikey ${APIKEY} --url ${AGENT_URL}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   +-----------------+----------------------------------------------+
   | NODE-PROPERTIES | VALUES                                       |
   +-----------------+----------------------------------------------+
   | VPNPublicKey    | ryKzTUEHcOy6b2CGSbK8zOpqTl/eTlpQZgIxH7R3twY= |
   | VPNIp           | 10.10.10.2/24                                |
   | WGPort          | 6666                                         |
   +-----------------+----------------------------------------------+

   command completed successfully.
   ```
   {: screen}

- The `vpn node delete` command
   ```sh
   ./hpnet node delete --apikey ${APIKEY} --url ${AGENT_URL} --admin-priv-key ${ADMIN_PRIV_KEY}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   command completed successfully.
   ```
   {: screen}

- The `vpn peer list` command
   ```sh
   ./hpnet peer list --apikey ${APIKEY} --url ${AGENT_URL}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   +--------------------------------------------------------------+---------------+------------------+
   | PUBLICKEY                                                    | VPNIP         | ENDPOINT         |
   +--------------------------------------------------------------+---------------+------------------+
   | ckZCVkZwaVpxUmtVZ2JWQUVPdy9kNTRqcHhOenk3TG1Nazh0WDVaYmJFOD0= | 10.10.10.2/32 | 172.17.0.2:6666  |
   +--------------------------------------------------------------+---------------+------------------+

   command completed successfully.
   ```
   {: screen}

- The `vpn peer delete` command
   ```sh
   ./hpnet peer delete --apikey ${APIKEY} --url ${AGENT_URL} --admin-priv-key ${ADMIN_PRIV_KEY} --peer-vpn-ip ${PEER_VPN_IP}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   command completed successfully.
   ```
   {: screen}

- To refresh the admin key pair (update the adminkey with an auto-generated admin keypair), run the following command.
   ```sh
   ./hpnet adminkey update --apikey ${APIKEY} --url ${AGENT_URL} --admin-priv-key ${ADMIN_PRIV_KEY}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   +------------+--------------------------------------------------------------------+
   | KEYTYPE    | FILE                                                               |
   +------------+--------------------------------------------------------------------+
   | PrivateKey | /Users/sheryl/.hpnet-cfg/b8a10ea5-0b62-24a5-09b8-48d011c0c75b.priv |
   | PublicKey  | /Users/sheryl/.hpnet-cfg/b8a10ea5-0b62-24a5-09b8-48d011c0c75b.pub  |
   +------------+--------------------------------------------------------------------+

   An automatically generated keypair is stored locally and configured to node remotely, please keep the keypair carefully!!!

   command completed successfully.
   ```
   {: screen}

   Note: Once you refresh the admin key pair, you must use the new admin private key which is displayed as `PrivateKey` in the result table, as the value of `--admin-priv-key` to execute secure network agent CLI commands.


- To refresh the VPN node key pair for HPVS_A, run the following command.
   ```sh
   ./hpnet node set --apikey ${APIKEY} --url ${AGENT_URL_A} --admin-priv-key ${ADMIN_PRIV_KEY_A} --generate-keypair
   ```
   {: pre}

   where `--generate-keypair` is used to generate a new keypair for the node.

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   +-----------------+----------------------------------------------+
   | NODE-PROPERTIES | VALUES                                       |
   +-----------------+----------------------------------------------+
   | VPNPublicKey    | ryKzTUEHcOy6b2CGSbK8zOpqTl/eTlpQZgIxH7R3twY= |
   | VPNIp           | 10.10.10.2/24                                |
   | WGPort          | 6666                                         |
   +-----------------+----------------------------------------------+

   command completed successfully.
   ```
   {: screen}

- To update the VPN public key in trusted peer list for HPVS_B, run the following command.
   ```sh
   export PEER_VPN_PUBLIC_KEY_B=<updated_VPNPublicKey_of_VPN_node_for_HPVS_A>
   ./hpnet peer update --apikey ${APIKEY} --url ${AGENT_URL_B} --admin-priv-key ${ADMIN_PRIV_KEY_B} --peer-vpn-ip ${PEER_VPN_IP_B} --peer-endpoint ${PEER_ENDPOINT_B} --peer-vpn-pub-key ${PEER_VPN_PUBLIC_KEY_B}
   ```
   {: pre}

   The following snippet shows an example output when the command has completed successfully.
   ```sh
   command completed successfully.
   ```
   {: screen}

## Parameter specification
{: #param_limit}

The following table lists the specifications that apply to some of the parameters of the CLI commands.

| Parametes     | Specification   |
| ----- | --------- |
| url | A valid endpoint < hpvs-ip >:< agent-port >. The default port for the agent is 5566, so the URL should be < hpvs-ip >:5566 |
| vpnip     |  An ip address (a.b.c.d) or CIDR format ip address (a.b.c.d/n). If subnet is not specified, by default it is /24     |
| wgport (wireguard port)   | A value between 1024 - 65535          |
| peer-vpn-ip    | An ip address (a.b.c.d) or CIDR format ip address (a.b.c.d/n). If subnet is not specified, by default it is /32 |
| peer-endpoint    |  A valid endpoint < hpvs-ip >:< wgport >   |
{: caption="Table 1. Parameter specification" caption-side="bottom"}



Ensure that you specify the correct secure network management agent URL when you run the command to avoid inadvertent security breaches and potential attacks if an incorrect URL is used. To get the correct IP address of the virtual server instance, follow these [instructions](https://test.cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs).
{: note}  








