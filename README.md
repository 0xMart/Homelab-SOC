# Homelab-SOC
SOC home lab for learning and practicing SOC analyst skills: log collection, detection, threat hunting, incident response, and automation.

## 🧩 Architecture
```mermaid
graph TB
    subgraph lab["🏠 Home Lab Environment"]
        subgraph attack["Attack Infrastructure"]
            kali["🐉 Kali Linux<br/>Penetration Testing"]
        end
        
        subgraph targets["Target Systems"]
            win["💻 Windows<br/>Endpoint"]
            linux["🐧 Linux<br/>Server"]
        end
        
        subgraph monitoring["Security Monitoring"]
            splunk["🔍 Splunk Enterprise<br/>SIEM Platform"]
        end
    end
    
    kali -.->|Attacks| win
    kali -.->|Attacks| linux
    win -->|Logs| splunk
    linux -->|Logs| splunk
    splunk -->|Analysis| analyst["👤 Security Analyst<br/>(Me)"]
    
    style kali fill:#1a1a1a,stroke:#00ff00,stroke-width:3px,color:#fff
    style win fill:#0078d4,stroke:#005a9e,stroke-width:3px,color:#fff
    style linux fill:#ff6c00,stroke:#cc5500,stroke-width:3px,color:#fff
    style splunk fill:#00b388,stroke:#008f6c,stroke-width:3px,color:#fff
    style analyst fill:#e74c3c,stroke:#c0392b,stroke-width:3px,color:#fff
```
