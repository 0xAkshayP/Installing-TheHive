# Installing-TheHive-On-Debian/Ubuntu

# 🚀 TheHive Installation and Overview

TheHive is a comprehensive 4-in-1 Security Incident Response Platform (SIRP) designed for:

- Security Operations Centers (SOCs)
- Computer Security Incident Response Teams (CSIRTs)
- Computer Emergency Response Teams (CERTs)
- Information security professionals

TheHive provides tools to streamline incident response workflows, enhance collaboration, and empower information security practitioners in investigating and mitigating security threats.

## ✨ Key Features

- **Integration with MISP**: Seamless collaboration with the Malware Information Sharing Platform (MISP).
- **Real-Time Collaboration**: Live stream updates on cases, tasks, observables, and Indicators of Compromise (IOCs).
- **Efficient Task Management**: Special notifications enable efficient task handling, with previews and imports from sources like emails, CTI providers, and SIEMs.
- **Customizable Templates**: Create and manage cases and tasks with flexible templates, custom fields, and metrics.
- **Evidence Management**: Record progress, attach files securely, and import password-protected ZIP archives.
- **Observables Management**: Manage observables individually or in bulk, with import options from MISP events and alerts.
- **Threat Intelligence Integration**: Leverage Cortex analyzers and responders to accelerate investigations and enrich threat intelligence.

## 🏛️ Architecture Overview

TheHive can be deployed either as a single server or as a clustered setup depending on your environment. It consists of three core components:

- **Application Layer**: TheHive application server.
- **Database Layer**: Apache Cassandra (version 4.x).
- **Indexing Layer**: Elasticsearch (supports versions 7.x and 8.x).
- **File Storage**: Local file system for standalone setups, or NFS/S3 MINIO for clustered environments.

### 🔧 Cluster Support

TheHive supports clustered environments with virtual IPs and load balancers to scale horizontally for optimal performance.

## 📋 Prerequisites

Before installing TheHive, ensure the following system dependencies are met:

- Linux server (Debian/Ubuntu/CentOS preferred)
- Minimum 4 CPU cores, 8 GB RAM, 50 GB disk space (production use)
- Root or sudo privileges

## 📦 Installation


To install TheHive, follow the [official installation guide](https://docs.strangebee.com/thehive/installation/step-by-step-installation-guide/#thehive-installation-and-configuration).

 #OR

 Follow The Steps Below

# TheHive Installation Guide

This document provides step-by-step instructions to install **TheHive**, a comprehensive Security Incident Response Platform (SIRP). Follow the steps below to prepare your system, install necessary dependencies, and configure TheHive.

## 🛠 Dependencies

Before installing TheHive, make sure you install the required dependencies for your system.

### For **DEB-based** Systems (e.g., Ubuntu, Debian)
1. Open a terminal window.
2. Run the following command to install the necessary dependencies:

 ```bash
apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl software-properties-common python3-pip lsb-release
```

Ensure that all dependencies are successfully installed before proceeding with the TheHive installation process.

## ☕ Java Installation 

1. Open a terminal window.
2. Execute the following commands:

```
wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg
echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
sudo apt update
sudo apt install java-common java-11-amazon-corretto-jdk
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment
export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"

```

## 📦 Apache Cassandra Installation
TheHive uses Apache Cassandra for database storage. Supported version: 4.0.x.

Install Apache Cassandra (example for Debian/Ubuntu):
Installation#

DEB
1. Add Apache Cassandra repository references:
    Download Apache Cassandra repository keys using the following command:
```
wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg
```

   Add the repository to your system by appending the following line to the /etc/apt/sources.list.d/cassandra.sources.list file. This file may not exist, and you may need to create it.

   ```
echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
```
2. Once the repository references are added, update your package index and install Cassandra using the following command:
   ```
   sudo apt update
   sudo apt install cassandra
   ```
   
   By default, data is stored in /var/lib/cassandra. Set appropriate permissions for this directory to avoid any issues with data storage and access.

## ⚙️Configuration
Configure Cassandra by modifying settings within the /etc/cassandra/cassandra.yaml file.

1.Locate the Cassandra configuration file:

 Navigate to the directory containing the Cassandra configuration file `/etc/cassandra/`.

2.Edit the cassandra.yaml file:

 Open the cassandra.yaml file in a text editor with appropriate permissions.

3.Configure cluster name:

 Set the cluster_name parameter to the desired name. This name helps identify the Cassandra cluster.

4.Configure listen address:

 Set the listen_address parameter to the IP address of the node within the cluster. Other nodes within the cluster use this address to communicate.

5.Configure RPC address:

 Set the rpc_address parameter to the IP address of the node to enable clients to connect to the Cassandra cluster.

6.Configure seed provider:

 Ensure the seed_provider section is properly configured. The seeds parameter should contain the IP addresses of the seed nodes in the cluster.

7.Configure directories:

 Set the directories for data storage, commit logs, saved caches, and hints per your requirements. Ensure that the specified directories exist and have appropriate permissions.

8.Save the changes:

 After making the necessary configurations, save the changes to the cassandra.yaml file.

## 🔥Start the service

1. Execute the following command to start the Cassandra service:
~~~
sudo systemctl start cassandra
~~~

2. Enable the Cassandra service to restart automatically after a system reboot:
~~~
sudo systemctl enable cassandra
~~~

3. Optional: If the Cassandra service was started automatically before configuring it, it's recommended to stop it, remove existing data, and restart it once the configuration is updated.
Execute the following commands:
~~~
sudo systemctl stop cassandra
sudo rm -rf /var/lib/cassandra/*
 ~~~

 # 📈Elasticsearch#
 
 TheHive uses Elasticsearch as its indexing engine.

Supported versions:

7.x

8.x

Example: Install Elasticsearch 7.x
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch
```

Enable and start Elasticsearch:
```
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

# Configuration
You can configure Elasticsearch by modifying settings within the /etc/elasticsearch/elasticsearch.yml file.

1. Locate the Elasticsearch configuration file:

Navigate to the directory containing the Elasticsearch configuration file `/etc/elasticsearch/`.

2. Edit the elasticsearch.yml file:

Open the `elasticsearch.yml` file in a text editor with appropriate permissions.

3. Configure HTTP and transport hosts:

Set the `http.host` and `transport.host` parameters to `127.0.0.1` or the desired IP address.

4. Configure cluster name:

Set the `cluster.name` parameter to the desired name. This name helps identify the Elasticsearch cluster.

5. Configure thread pool search queue size:

Set the `thread_pool.search.queue_size` parameter to the desired value, such as `100000`.

6. Configure paths for logs and data:

Set the `path.logs` and `path.data` parameters to the desired directories, such as `/var/log/elasticsearch` and `/var/lib/elasticsearch`, respectively.

7. Optional: Configure X-Pack security:

If you're not using X-Pack security, ensure that `xpack.security.enabled` is set to `false`.

8. Optional: Configure script allowed types:

If needed, set the `script.allowed_types` parameter to specify allowed script types.

9. Save the changes:

After making the necessary configurations, save the changes to the `elasticsearch.yml` file.

# 🔥Start the service

1. Execute the following command to start the Elasticsearch service:
```
sudo systemctl start elasticsearch
```

2. Enable the Elasticsearch service to restart automatically after a system reboot:

```
sudo systemctl enable elasticsearch
```

3. Optional: If the Elasticsearch service was started automatically before configuring it, it's recommended to stop it, remove existing data, and restart it once the configuration is updated.
Execute the following commands:

```
sudo systemctl stop elasticsearch
sudo rm -rf /var/lib/elasticsearch/*
```


# 🐝 TheHive Installation

This section provides detailed instructions for installing and configuring TheHive..

Installation


For Debian systems, use the following command:
```
wget -O- https://raw.githubusercontent.com/StrangeBeeCorp/Security/main/PGP%20keys/packages.key | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
```

Install TheHive package by using the following commands:
```
echo 'deb [arch=all signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.4 main' |sudo tee -a /etc/apt/sources.list.d/strangebee.list
sudo apt-get update
sudo apt-get install -y thehive
```

Configuration#
The setup provided with binary packages is tailored for a standalone installation, with all components hosted on the same server. At this point, it's crucial to fine-tune the following parameters as necessary:


Happy Incident Responding! 🎯
