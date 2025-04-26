# Installing-TheHive

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

##📦 Apache Cassandra Installation
TheHive uses Apache Cassandra for database storage. Supported version: 4.0.x.

Install Apache Cassandra (example for Debian/Ubuntu):
Installation#

DEB
1. Add Apache Cassandra repository references:
    Download Apache Cassandra repository keys using the following command:
```
wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg
```

Happy Incident Responding! 🎯
