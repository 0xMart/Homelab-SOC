# Homelab-SOC
SOC home lab for learning and practicing SOC analyst skills: log collection, detection, threat hunting, incident response, and automation.

## 🧩 Architecture
```mermaid
flowchart LR
    A["⚔️ Launch Attacks"] -->|Generate Logs| B["📊 Splunk SIEM"]
    B -->|Create Queries| C["🔎 Threat Detection"]
    C -->|Practice| D["🎓 SOC Skills"]
    
    style A fill:#34495e,stroke:#2c3e50,stroke-width:3px,color:#fff
    style B fill:#27ae60,stroke:#229954,stroke-width:3px,color:#fff
    style C fill:#f39c12,stroke:#e67e22,stroke-width:3px,color:#fff
    style D fill:#e74c3c,stroke:#c0392b,stroke-width:3px,color:#fff
```
