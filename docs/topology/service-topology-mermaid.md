---
type: diagram
area: services
status: active
updated: 2026-02-14
---
# Service Topology and Trust Boundaries

```mermaid
flowchart TB

  subgraph CLIENTS["Client Segments"]
    C10["Trusted Clients<br/>VLAN10"]:::trusted
    C30["Management Clients<br/>VLAN30"]:::mgmt
    C40["Guest Clients<br/>VLAN40"]:::guest
    C50["IoT Clients<br/>VLAN50"]:::iot
  end

  subgraph CTRL["Network Control Plane"]
    FW["OPNsense<br/>Gateway + Firewall"]:::edge
    DNS["Dnsmasq<br/>DHCP + DNS"]:::svc
    SW["MikroTik CSS"]:::core
    AP["UniFi U6 Pro"]:::wireless
    UC["UniFi Controller<br/>(temporary on Mac)"]:::svc
  end

  subgraph FUTURE["Planned Service Plane"]
    VPN["Tailscale / WireGuard"]:::vpn
    PX["Mini PC + Proxmox"]:::host
    RP["Reverse Proxy"]:::svc
    GF["Grafana"]:::svc
    JF["Jellyfin"]:::svc
    BW["Bitwarden (Cloud)<br/>or Vaultwarden (Self-Hosted)"]:::svc
    NAS["DIY NAS"]:::storage
  end

  C10 --> FW
  C30 --> FW
  C40 --> FW
  C50 --> FW

  FW --> DNS
  FW --> SW
  SW --> AP
  UC -. "manages" .-> AP

  VPN --> FW
  FW --> PX
  FW --> NAS
  PX --> RP
  PX --> GF
  PX --> JF
  C10 --> BW
  C30 --> BW
  NAS --> JF

  C40 -. "Mgmt GUI access: deny" .-> FW
  C50 -. "Mgmt GUI access: deny" .-> FW

  classDef edge fill:#eff6ff,stroke:#2563eb,color:#1e3a8a,stroke-width:2px;
  classDef core fill:#ecfeff,stroke:#0891b2,color:#164e63,stroke-width:2px;
  classDef wireless fill:#f0fdf4,stroke:#16a34a,color:#14532d,stroke-width:2px;
  classDef trusted fill:#f0f9ff,stroke:#0284c7,color:#0c4a6e;
  classDef mgmt fill:#f5f3ff,stroke:#7c3aed,color:#4c1d95;
  classDef guest fill:#fff1f2,stroke:#e11d48,color:#881337;
  classDef iot fill:#fefce8,stroke:#ca8a04,color:#713f12;
  classDef svc fill:#f8fafc,stroke:#475569,color:#1e293b;
  classDef host fill:#eef2ff,stroke:#4f46e5,color:#312e81;
  classDef storage fill:#ecfccb,stroke:#65a30d,color:#365314;
  classDef vpn fill:#fff7ed,stroke:#ea580c,color:#7c2d12;
```

## Notes
- Remote administration target model is VPN-first (not broad WAN exposure).
- Management GUIs should be reachable only from trusted admin sources.
- Final service placement will be pinned after Proxmox + NAS are online.
- Secrets management plan is Bitwarden cloud first; optional Vaultwarden self-host can be introduced later.
