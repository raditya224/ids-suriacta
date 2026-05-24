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
