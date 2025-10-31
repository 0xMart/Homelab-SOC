# Homelab-SOC
SOC home lab for learning and practicing SOC analyst skills: log collection, detection, threat hunting, incident response, and automation.

## 🧩 Architecture
```mermaid
flowchart TB
    subgraph endpoints["🖥️ Endpoints"]
        direction LR
        win["Windows"]
        linux["Linux"]
        kali["Attack Machine"]
    end

    subgraph siem["🔍 SIEM Platform"]
        direction TB
        collector["Log Collector"]
        storage["Data Storage"]
        analysis["Analysis & Visualization"]
    end

    subgraph soc["👤 SOC Analyst"]
        direction TB
        monitoring["Monitoring"]
        investigation["Investigation"]
        response["Incident Response"]
    end

    endpoints -->|Logs & Events| collector
    collector --> storage
    storage --> analysis
    analysis --> monitoring
    monitoring --> investigation
    investigation --> response

    style endpoints fill:#2c3e50,stroke:#34495e,stroke-width:2px,color:#ecf0f1
    style siem fill:#27ae60,stroke:#229954,stroke-width:2px,color:#ecf0f1
    style soc fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#ecf0f1
```
