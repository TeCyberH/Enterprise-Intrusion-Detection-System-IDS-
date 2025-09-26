# Enterprise Intrusion Detection System (IDS)

## 🛡️ Project Overview

This is a comprehensive, multi-layered Enterprise Intrusion Detection System built using Python and leveraging a scalable distributed architecture (Kafka, Elasticsearch, Redis). It employs advanced detection techniques, including signature matching, behavioral analysis (UEBA), and machine learning (ML), coupled with automated, context-aware incident response (IR) capabilities.

The primary goal of this IDS is to provide real-time threat detection and mitigation in a high-volume network environment.

### Key Features

* **Multi-Layered Detection:** Combines **Signature-based** (Regex), **Statistical Anomaly** (Isolation Forest), **Deep Learning** (Autoencoder), and **Behavioral Analysis (UEBA)**.
* **Scalable Architecture:** Uses **Kafka** for high-throughput event streaming and **Elasticsearch** for persistent, indexed storage.
* **Automated Response:** Executes real-time actions like **IP Blocking (iptables)** and **Host Isolation**, dynamically escalating based on asset criticality.
* **High-Performance Capture:** Utilizes **Scapy** for reliable, low-level packet capture and flow reconstruction.
* **Monitoring & Visualization:** Integrated with Kibana (via Elasticsearch) and Grafana for security operations center (SOC) dashboards.

## ⚙️ Architecture and Components

The system is deployed using Docker Compose, creating a robust, distributed environment:

| Component | Technology | Role |
| :--- | :--- | :--- |
| **IDS Core** | Python, Scapy, scikit-learn, Keras/TF | Main detection engine, event processing, and response orchestration. |
| **Event Stream** | Apache Kafka | Real-time buffer and messaging queue for raw and processed events. |
| **Data Storage** | Elasticsearch | Long-term, searchable storage for all security events. |
| **Dashboard** | Kibana / Grafana | Visualization, analytics, and SOC monitoring interface. |
| **Caching/State** | Redis | High-speed cache for session data, response history, and blacklists. |
| **Packet Capture** | Scapy / Raw Sockets | Network interface monitoring and raw packet parsing. |

## 🚀 Getting Started

### Prerequisites

You must have the following installed on your host machine:

1.  **Docker** and **Docker Compose**
2.  **Git** (for cloning the repository)

### Installation and Setup

1.  **Clone the Repository:**
    ```bash
    git clone [YOUR_REPO_URL]
    cd [your-project-directory]
    ```

2.  **Ensure File Structure:** Verify the following structure exists (create the missing directories):
    ```
    .
    ├── enterprise_ids.py         # Main application code
    ├── requirements.txt
    ├── Dockerfile
    ├── docker-compose.yml
    └── config/
        └── ids_config.yaml       # Configuration file
    ```

3.  **Deploy the Stack:**
    Build and start all services defined in `docker-compose.yml`. This must be run with sufficient permissions to allow the IDS Core container to access network interfaces (via `network_mode: host` and `CAP_ADD: NET_RAW`).
    ```bash
    docker-compose up --build -d
    ```

### Accessing the System

| Service | Host Port | Purpose |
| :--- | :--- | :--- |
| **Kibana** | `http://localhost:5601` | Event visualization and search. |
| **Grafana** | `http://localhost:3000` | System health and KPI dashboards. |
| **Redis** | `localhost:6379` | Response data cache. |

## 📋 Configuration (`config/ids_config.yaml`)

The primary configuration file manages all operational aspects:

| Section | Key Parameter | Description |
| :--- | :--- | :--- |
| `network` | `interfaces` | List of network interfaces to monitor (e.g., `eth0`). |
| `network` | `bpf_filter` | Berkeley Packet Filter string (e.g., `"port 80 or port 443"`). |
| `detection` | `algorithms` | List of ML algorithms to enable (`isolation_forest`, `autoencoder`, etc.). |
| `response` | `critical_assets`| List of internal IP addresses considered high-value targets (triggers escalation). |
| `kafka` / `es` | `bootstrap_servers` / `hosts` | Connection details for the data backbone services. |

## 💻 Running & Testing

### Viewing Real-time Logs

Monitor the main IDS core application logs to see detected events and system status:

```bash
docker logs -f ids-core

Triggering a Test Event
To test signature detection manually, you can simulate traffic that matches one of the simple built-in rules (e.g., the SQL Injection rule SQL-001 or Brute Force BRU-001).

Generate Test Data (Requires a simulated network connection being monitored by the IDS).

Verify Response Look for a CRITICAL or HIGH event in the ids-core logs, indicating that BLOCK_IP or QUARANTINE actions were attempted.

🤝 Contribution
We welcome contributions! Please feel free to open issues or submit pull requests for:

Adding more sophisticated ML models.

Integrating with popular HIDS (e.g., Wazuh/OSSEC) event streams.

Improving the ResponseOrchestrator logic.
