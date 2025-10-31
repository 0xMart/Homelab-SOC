# Homelab-SOC
SOC home lab for learning and practicing SOC analyst skills: log collection, detection, threat hunting, incident response, and automation.

## 🧩 Architecture
```mermaid
graph LR
    subgraph sources["Data Sources"]
        W[Windows]
        L[Linux]
        K[Kali]
    end
    
    subgraph siem["SIEM"]
        S[Splunk]
    end
    
    subgraph analysis["Analysis"]
        A[Me]
    end
    
    W & L -->|logs| S
    K -.->|attacks| W
    K -.->|attacks| L
    S -->|investigate| A
    
    style W fill:#0078d4,stroke:#005a9e,stroke-width:2px,color:#fff
    style L fill:#ff6c00,stroke:#cc5500,stroke-width:2px,color:#fff
    style K fill:#000,stroke:#00ff00,stroke-width:2px,color:#0f0
    style S fill:#00b388,stroke:#008f6c,stroke-width:3px,color:#fff
    style A fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#fff
```
