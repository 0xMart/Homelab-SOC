# Homelab-SOC
SOC home lab for learning and practicing SOC analyst skills: log collection, detection, threat hunting, incident response, and automation.

## 🧩 Architecture
```mermaid
flowchart LR
    A[("🎯<br/>Attack<br/>Surface")]
    B[("📊<br/>SIEM<br/>Platform")]
    C[("🔍<br/>Threat<br/>Hunting")]
    
    A -->|Security Events| B
    B -->|Detection Rules| C
    
    style A fill:#24292e,stroke:#586069,stroke-width:4px,color:#f0f6fc
    style B fill:#238636,stroke:#2ea043,stroke-width:4px,color:#ffffff
    style C fill:#0969da,stroke:#1f6feb,stroke-width:4px,color:#ffffff
```
