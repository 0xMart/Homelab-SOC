# Homelab-SOC
SOC home lab for learning and practicing SOC analyst skills: log collection, detection, threat hunting, incident response, and automation.

## 🧩 Architecture
```mermaid
flowchart TB
    subgraph Host_Network["Host-Only VMnet (isolated)"]
        direction TB
        Win["Windows Host\n(Splunk Universal Forwarder)"]
        Kali["Kali Linux\n(Attacker) (TODO)"]
        Ubuntu["Ubuntu Desktop\n(Endpoint) (TODO)"]
    end

    subgraph Splunk_Server["Debian 13 - Splunk Enterprise\n192.168.84.130"]
        direction TB
        SplunkWeb["Splunk Web UI :8000"]
        SplunkRecv["Splunk Receiving :9997"]
        Indexes["Indexes: windows_logs, linux_logs, network_logs"]
    end

    %% Forwarding / Flows
    Win -->|Forward logs :9997| SplunkRecv
    Ubuntu -.->|Forward logs UF/Filebeat TODO| SplunkRecv
    Kali -.->|Attack traffic TODO| Win
    Kali -.->|Attack traffic TODO| Ubuntu

    %% Splunk data flow
    SplunkRecv --> Indexes
    Indexes --> SplunkWeb
    SplunkWeb -->|Dashboards & Alerts| Analyst["SOC Analyst"]

    classDef simple fill:#f8f8fa,stroke:#111,stroke-width:1px;
    class Host_Network,Splunk_Server simple;
```
