# 🧭 Homelab IP Address Plan

**Network:** `192.168.178.0/24`  
**Gateway:** `192.168.178.1` (Fritz!Box)

---

## 🎯 Design Rationale

This IP plan keeps the **homelab network structured, predictable, and easy to reason about** — even though the Fritz!Box only supports a single subnet.

The idea is to use **clear IP ranges** that instantly reveal the purpose of any device:

- **Low IPs (`.2–.19`)** → Core infrastructure (servers, Kubernetes nodes, NAS, DNS)  
- **Middle range (`.100–.200`)** → Dynamic DHCP clients (phones, laptops, IoT, guests)  
- **High range (`.230–.239`)** → Virtual service IPs (MetalLB LoadBalancer outputs)

This ensures that any IP’s role can be identified instantly — and avoids collisions between static and dynamic devices.

---

## 🧩 IP Range Allocation

| Range | Purpose | Examples | Notes |
|--------|----------|-----------|-------|
| `192.168.178.1` | Gateway / Router | Fritz!Box | Default route |
| `192.168.178.2–19` | Core infrastructure | Kubernetes nodes, NAS, Pi-hole | Static IPs — real physical or VM hosts |
| `192.168.178.20–99` | Reserved static range | Proxmox VMs, management tools | Optional static space |
| `192.168.178.100–200` | DHCP pool | Phones, laptops, IoT, guests | Assigned automatically by Fritz!Box |
| `192.168.178.201–229` | Expansion / testing | Temporary lab VMs or experiments | Manual assignment |
| `192.168.178.230–239` | MetalLB service pool | Ingress, dashboards, internal apps | Virtual LoadBalancer IPs exposed to LAN |
| `192.168.178.240–254` | Spare / future | Future nodes or sandboxing | Reserved capacity |

---

## ⚙️ Why This Split?

### 🧠 1. Clarity
Every IP range has a **specific, easily recognizable purpose** — you can spot whether a device is part of your infrastructure or a random guest device instantly.

### 🔒 2. Safety
Static and dynamic addresses are **completely separated**, preventing DHCP conflicts and simplifying troubleshooting.

### ⚡ 3. Scalability
Plenty of room for additional nodes, VMs, and service IPs as your homelab grows.

### 🧱 4. Compatibility
Works 100% with **Fritz!Box** — no VLANs or extra routers required.  
You control everything by simply shrinking the DHCP range and assigning static IPs outside it.

---

## 🧰 Example Assignments

| Device | Role | IP | Notes |
|---------|------|----|-------|
| Fritz!Box | Router / Gateway | `192.168.178.1` | Default gateway |
| K8s Master | Control plane | `192.168.178.2` | Static IP |
| K8s Worker 1 | Node | `192.168.178.3` | Static IP |
| Proxmox | Hypervisor | `192.168.178.10` | Manages VMs / containers |
| Traefik Ingress | MetalLB service | `192.168.178.230` | LoadBalancer endpoint |
| Grafana | MetalLB service | `192.168.178.231` | Dashboard access |

---

## ✅ Summary

This refined IP plan provides:

- A **logical, human-readable structure** for all devices.  
- Low IPs reserved for **critical homelab infrastructure**.  
- A high IP range dedicated to **MetalLB and virtual services**.  
- Full **compatibility with Fritz!Box** networking limitations.  
- Instant recognition: if you see an IP in `.100–.200`, you know it’s not part of your infrastructure.
