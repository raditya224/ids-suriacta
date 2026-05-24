# 🛡️ IDS-Suricata (ELK Integration)

Documentation and pipeline setups for integrating Suricata Intrusion Detection System (IDS) with the ELK Stack to achieve centralized network security telemetry and visualization.

---

## 🏗️ Infrastructure Specification

The environment and resource allocations deployed for this implementation:

* **Operating System**   : `Ubuntu 22.04.5 LTS` (Deployed on 1 LXC Container)
* **Resource Allocation** :
  * **CPU**     : 3 Cores
  * **RAM**     : 5 GB
  * **Storage** : 10 GB
* **Network & Time**     : `Asia/Jakarta (WIB)` *Synced via Hypervisor Host Clock*

---

## 🛠️ System Architecture & Data Pipeline

The telemetry data flow from the network interface to the visualization dashboard is processed through the following pipeline:

```text
  [ Network Traffic ] 
          │
          ▼  (Promiscuous Mode / Port Mirroring)
    [ Suricata IDS ] ──> Generates /var/log/suricata/eve.json
          │
          ▼  (Shipped via Filebeat / Logstash)
     [ ELK Stack ]  ──> Elasticsearch (Storage & Indexing)
          │
          ▼
    [ Kibana UI ]   ──> Security Monitoring Dashboard
```

## 🚀 Quick Deployment Reference

Follow the steps below to deploy and configure the system:

### 1. Suricata Installation & Timezone Sync

You can perform the installation manually using the commands below, or automate the process using the provided deployment script: `install_suricata.sh`.

#### Option A: Manual Installation & Configuration

First, synchronize the system timezone to ensure accurate alert timestamps, then install Suricata and its dependencies:

```bash
# Configure system timezone to WIB (UTC+07:00)
sudo timedatectl set-timezone Asia/Jakarta

# Install prerequisites and register the stable repository
sudo apt update
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:oisf/suricata-stable

# Update package lists and install Suricata along with jq for JSON processing
sudo apt update
sudo apt install -y suricata jq
