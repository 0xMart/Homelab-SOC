# Homelab-SOC
SOC home lab for learning and practicing SOC analyst skills: log collection, detection, threat hunting, incident response, and automation.

## 🧩 Architecture
```mermaid
flowchart TB
    subgraph layer3["🎯 Analysis Layer"]
        me["Security Analyst"]
    end
    
    subgraph layer2["⚙️ Processing Layer"]
        splunk["Splunk SIEM"]
    end
    
    subgraph layer1["📡 Data Sources"]
        direction LR
        kali["Kali<br/>(Attacker)"]
        windows["Windows"]
        linux["Linux"]
    end
    
    kali -.->|exploits| windows
    kali -.->|attacks| linux
    
    windows -->|events| splunk
    linux -->|logs| splunk
    
    splunk -->|dashboards| me
    
    style layer1 fill:#2c3e50,stroke:#34495e,stroke-width:2px,color:#ecf0f1
    style layer2 fill:#27ae60,stroke:#229954,stroke-width:2px,color:#ecf0f1
    style layer3 fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#ecf0f1
    style kali fill:#000,stroke:#00ff00,stroke-width:3px,color:#0f0
```
