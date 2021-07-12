---

copyright:
  years: 2021
lastupdated: "2021-06-04"

subcollection: hp-virtual-servers

keywords: virtual server instance, instance, logging, logs, logDNA, virtual server

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


# Monitoring logs
{: #monitoring}

You can monitor many kinds of logs of the {{site.data.keyword.hpvs}} instances, for example, web server, or application logs. To enable the collection of such logs, you must use IBM Log Analysis with LogDNA on the {{site.data.keyword.hpvs}} instance.

{: shortdesc}

## Prerequisites
{: #log_pre}

You must deploy a {{site.data.keyword.hpvs}} instance on IBM Cloud. Follow these [instructions](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-provision) to provision a virtual server. To log in to the virtual server, follow these [instructions](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-connect_vs).


## Deploying IBM Log Analysis with LogDNA
{: dep_dna}

In general, to deploy IBM Log Analysis with LogDNA, follow these [instructions](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-provision). You can configure platform logs when logging is active. If you want to configure a LogDNA agent for each log source that you want to monitor, follow these [instructions](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-config_agent_linux) for configuring a logging agent on Linux Ubuntu or Debian. 

For {{site.data.keyword.hpvs}}, to configure a LogDNA agent for the IBM Z platform (s390x architecture), you must complete these steps:


1. To install dependencies and update node, run:
   ```
   apt-get update
   ```
   {: pre}
   ```
   apt-get install -y git npm curl wget
   ```
   {: pre}
   ```
   npm install -g n
   ```
   {: pre}
   ```
   n lts
   ```
   {: pre}
   ```
   PATH="$PATH"
   ```
   {: pre}
   ```
   node -v
   ```
   {: pre}
2. Get the source code for the logging agent from [this GitHub repository](https://github.com/logdna/logdna-agent).
   - To clone the repository, run:
     ```
     git clone https://github.com/logdna/logdna-agent.git
     ```
     {: pre}
   - To install the agent from the source, run:
     ```
     cd logdna-agent && npm install
     ```
     {: pre}
3. Complete the following steps to configure the LogDNA agent:    
   - To get the ingestion key, follow these [instructions](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-ingestion_key), where the `INGESTION_KEY` contains the ingestion key that is active for the IBM Log Analysis with the LogDNA instance that you are configuring to forward logs.
   - To get the values of `APIHOST` and `LOGHOST`, follow these [instructions](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-config_agent_linux). You also can open the logging service by using the dashboard to get the values of `APIHOST` and `LOGHOST`. Run the following commands:
     ```
     node index.js -k <YOUR LOGDNA INGESTION KEY>
     ```
     {: pre}
     ```
     node index.js -d /var/log
     ```
     {: pre}
     ```
     node index.js -t logging
     ```
     {: pre}
     ```
     node index.js -s <APIHOST>   
     ```
     {: pre}

     {:note}
     For example, `node index.js -s LOGDNA_APIHOST=api.us-south.logging.cloud.ibm.com`
     ```
     node index.js -s <LOGHOST>
     ```
     {: pre}

     {:note}
     For example, `node index.js -s LOGDNA_LOGHOST=logs.us-south.logging.cloud.ibm.com`
4. To start the LogDNA agent, run:
   ```
   node index.js
   ```
   {: pre}


## Example -Checking the logs by using SSH log in and log out
{: #logging_example}

You can configure the logs that you want to monitor with the agent so that the logs can be captured by using the web user interface.

1. To check the settings of the agent, run:
   ```
   node index.js -l
   hostname       = af65e44fb2e6
   logdir         = [ /var/log, /var/log/audit.log ]
   key            = 46f2c83ba4cd5cfec500154181b9d95e
   tags           = logging
   LOGDNA_APIHOST = api.us-south.logging.cloud.ibm.com
   LOGDNA_LOGHOST = logs.us-south.logging.cloud.ibm.com

   ```
   {: codeblock}
2. Open the logging web user interface to check the log information, and then log in or log out of the {{site.data.keyword.hpvs}} instance many times, and update with the live button. You can view the logs by using the web user interface. You can follow these [instructions](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-view_logs) for information about how to view the logs by using the web user interface.
