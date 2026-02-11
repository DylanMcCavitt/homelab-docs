---
type: diagram
area: network
status: active
updated: 2026-02-11
---
# Physical Network Topology

```mermaid
flowchart LR

  subgraph WAN["WAN Edge"]
    ISP["Internet / ISP"]:::internet --> MOD["Spectrum Modem"]:::edge --> OPN["Protectli + OPNsense<br/>igc0: WAN | igc3: LAN trunk"]:::edge
  end

  subgraph CORE["Core Switching"]
    OPN -- "802.1Q trunk<br/>VLAN 1,10,20,30,40,50" --> SW["MikroTik CSS (SwOS)"]:::core
    SW -- "Port6 access VLAN1<br/>(rescue)" --> R["Rescue Admin Host"]:::mgmt
    SW -- "Port8 temporary access<br/>(VLAN20/VLAN30 tests)" --> T["Mac Test Client"]:::mgmt
  end

  subgraph WIFI["Wireless Distribution"]
    SW -- "Port3 trunk<br/>VLAN1,10,40,50" --> AP["UniFi U6 Pro"]:::wireless
    AP --> V10["VLAN10 Trusted Clients"]:::trusted
    AP --> V40["VLAN40 Guest Clients"]:::guest
    AP --> V50["VLAN50 IoT Clients"]:::iot
  end

  subgraph WIRED["Wired Segments (planned cutover)"]
    SW --> V20["VLAN20 Servers / NAS / Mini PC"]:::servers
    SW --> V30["VLAN30 Management Devices"]:::mgmt
  end

  classDef internet fill:#fff7ed,stroke:#ea580c,color:#7c2d12,stroke-width:2px;
  classDef edge fill:#eff6ff,stroke:#2563eb,color:#1e3a8a,stroke-width:2px;
  classDef core fill:#ecfeff,stroke:#0891b2,color:#164e63,stroke-width:2px;
  classDef wireless fill:#f0fdf4,stroke:#16a34a,color:#14532d,stroke-width:2px;
  classDef trusted fill:#f0f9ff,stroke:#0284c7,color:#0c4a6e;
  classDef servers fill:#f8fafc,stroke:#475569,color:#1e293b;
  classDef mgmt fill:#f5f3ff,stroke:#7c3aed,color:#4c1d95;
  classDef guest fill:#fff1f2,stroke:#e11d48,color:#881337;
  classDef iot fill:#fefce8,stroke:#ca8a04,color:#713f12;
```

## Notes
- Port6 stays as the rescue path until all cutovers are validated.
- Port8 is temporary validation only.
- AP management remains on native LAN during transition.
