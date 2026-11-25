### 🏠 w_lab
My Kubernetes homelab: more expensive and less reliable than the cloud, but way more fun!


## 🤖 Motivation
The goal of this project is to give all of my networking toys a home that fits in the attic crawl-space or on a self in the basement.

Eventually, this project will culminate with my own private cloud and self-hosted kubernetes cluster, so I would like to keep performance and upgradability in mind. Running K3s in production with Flux GitOps.

## 🔧 Hardware
| Piece	| What it is | Cost (*including 6% sales tax) |
|-------|------------| --------------------------|
| **Router/Firewall** | UniFi UCG-Fiber | $295.74* |
| **Cellular Failover Router** | NETGEAR Nighthawk M1 | no longer sold |
| **Access Point** | UniFi U7-Pro-Wall | $210.94* |
| **Switch A** | UniFi USW-Pro-XG-8-PoE | $528.94* |
| **Switch B** | UniFi USW-Ultra | $136.74* |
| **Patch Cables** | Assorted UniFi Patch Cables | $68.86* |
| **Patch Cables** | Assorted Monoprice Patch Cables | $87.92* |
| **Patch Panel A** | DeskPi 12 Port CAT6 Network Patch Panel | $24.37* |
| **Patch Panel B** | Rapink Mini 12 Port Cat6A Patch Panel | $29.68* |
| **Compute (cluster)** | 3x Dell OptiPlex 7060 (i5-8500T CPU, 16GB RAM, 2.5GbE NIC) | $340.45 |
| **GPU Workstation / Agent Node** _(Jan 2026)_ | CyberPowerPC Winter 9950X: AMD Ryzen 9 9950X3D, NVIDIA RTX 5080 16GB, 32GB DDR5, 2TB NVMe, Asetek 360mm AIO (also serves as K3s agent node `wgpu01`) | $3,176.08* |
| **NAS** | Synology DS923+ (2x Seagate IronWolf 8TB RAID1, 2x 500GB WD Red SN700 NVMe, 10GbE NIC) | $1,255* |
| **UPS**	| Tripp Lite 600VA 300W UPS - BC600RNC | $155.09* |
| **PDU**	| 4 Outlet PDU | $14.30* |
| **USB Power**	| 300 W USB‑C charging station | $24.78* |
| **USB C Cables**	| 3x 60W USB-C to USB-C Cables | $10.59* |
| **Misc. Devices** | Philips Hue Bridge | included with lights |
| **Misc. Devices** | Raspberry Pi 2 B | no longer sold |
| **Misc. Devices** | HDHomeRun EXTEND | no longer sold |
| **Mini‑rack** | DeskPi RackMate T2 (10″ 12U) | $195.03* |
| **Mini-rack Accessories** | T2 Metal Shelf, 0.5U Brush Cable Management, 1U Blank, 2x 2U Blank, Mounting Hardware | $94.51* |
| Total | One bad ass closet that'll actually fit in a closet (plus a workstation that won't) | $6,649.02* |

_Rack and networking priced as of May 2025; GPU workstation purchased January 2026._



## 🧠 Software Stack
This homelab runs a complete Kubernetes infrastructure with GitOps automation:

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Kubernetes** | K3s | Lightweight Kubernetes distribution |
| **GitOps** | Flux v2 | Automated deployment and configuration management |
| **Ingress** | Traefik | HTTP/HTTPS routing and load balancing |
| **LoadBalancer** | MetalLB | LoadBalancer implementation for bare metal |
| **Storage** | Synology CSI | Integration with NAS for persistent storage |
| **Certificates** | cert-manager | Automated TLS certificate management |
| **Secrets** | Sealed Secrets | Encrypted secrets management for GitOps |
| **Database** | CloudNativePG + Redis | Production-ready PostgreSQL operator + caching layer |
| **Monitoring** | Prometheus + Grafana | Metrics collection, visualization, and dashboards |
| **Automation** | Renovate + n8n | Dependency updates (CI-gated automerge) and workflow automation |
| **Logs** | Loki + Grafana Alloy | 14-day queryable log history for every pod |
| **Alerting** | Alertmanager + ntfy | Push alerts to phones, plus a healthchecks.io dead-man's switch that reports whole-lab outages from outside |
| **Backups** | CNPG volumeSnapshots + snapscheduler | Nightly NAS-level snapshots of databases and stateful app volumes |
| **Mesh VPN** | Tailscale Operator | Secure cluster access, API proxy, and Tailscale Ingresses |
| **GPU Compute** | NVIDIA Device Plugin | GPU scheduling for LLM inference and compute workloads on RTX 5080 |
| **Security Scanning** | Trivy Operator | Continuous vulnerability scanning of all running container images |
| **Node Management** | Kured + System Upgrade Controller | Automated node reboots and K3s version upgrades |
| **Config Reload** | Reloader | Rolling updates on Secret/ConfigMap changes |
| **Volume Snapshots** | external-snapshotter | CSI volume snapshot support for backup policies |
| **CI/CD** | GitHub ARC + BuildKit | In-cluster ephemeral Actions runners and a shared remote BuildKit for container builds |

## ⚡ Applications & Services
The cluster hosts a variety of self-hosted applications:

**Media & Entertainment:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/plex.svg" width="16" height="16"> [Plex Media Server](https://www.plex.tv/)** - Streaming with Intel QuickSync hardware transcoding ([Install Guide](https://support.plex.tv/articles/200264746-quick-start-step-by-step-guides/))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/radarr.svg" width="16" height="16"> [Radarr](https://radarr.video/)** - Movie collection manager and downloader ([GitHub](https://github.com/Radarr/Radarr))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/sonarr.svg" width="16" height="16"> [Sonarr](https://sonarr.tv/)** - TV series collection manager and downloader ([GitHub](https://github.com/Sonarr/Sonarr))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/lidarr.svg" width="16" height="16"> [Lidarr](https://lidarr.audio/)** - Music collection manager and downloader ([GitHub](https://github.com/Lidarr/Lidarr))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/bazarr.svg" width="16" height="16"> [Bazarr](https://www.bazarr.media/)** - Subtitle management for movies and TV shows ([GitHub](https://github.com/morpheus65535/bazarr))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/ombi.svg" width="16" height="16"> [Ombi](https://ombi.io/)** - Media request management system ([GitHub](https://github.com/Ombi-app/Ombi))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/tautulli.svg" width="16" height="16"> [Tautulli](https://tautulli.com/)** - Plex analytics, stream monitoring, and history tracker ([GitHub](https://github.com/Tautulli/Tautulli))
- **<img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/svg/commafeed.svg" width="16" height="16"> [CommaFeed](https://www.commafeed.com/)** - Self-hosted RSS reader and aggregator ([GitHub](https://github.com/Athou/commafeed))

**Home Automation:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/home-assistant.svg" width="16" height="16"> [Home Assistant](https://www.home-assistant.io/)** - Complete home automation platform ([Installation Guide](https://www.home-assistant.io/installation/) | [GitHub](https://github.com/home-assistant/core))

**Monitoring & Observability:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/prometheus.svg" width="16" height="16"> [Prometheus](https://prometheus.io/)** - Metrics collection and alerting with 365-day retention ([Installation Guide](https://prometheus.io/docs/prometheus/latest/installation/) | [GitHub](https://github.com/prometheus/prometheus))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/grafana.svg" width="16" height="16"> [Grafana](https://grafana.com/)** - Visualization dashboards with persistent storage ([Installation Guide](https://grafana.com/docs/grafana/latest/setup-grafana/installation/) | [GitHub](https://github.com/grafana/grafana))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/prometheus.svg" width="16" height="16"> [AlertManager](https://prometheus.io/docs/alerting/latest/alertmanager/)** - Alert routing and management ([GitHub](https://github.com/prometheus/alertmanager))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/prometheus.svg" width="16" height="16"> [Blackbox Exporter](https://prometheus.io/docs/guides/multi-target-exporter/)** - External endpoint monitoring and health checks ([GitHub](https://github.com/prometheus/blackbox_exporter))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/unpoller.svg" width="16" height="16"> [Unpoller](https://unpoller.com/)** - UniFi network telemetry and Prometheus metrics ([GitHub](https://github.com/unpoller/unpoller))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/victoriametrics.svg" width="16" height="16"> [VictoriaMetrics](https://victoriametrics.com/)** - Long-term time-series database for WARN business KPIs ([GitHub](https://github.com/VictoriaMetrics/VictoriaMetrics))

**Dashboard & Management:**
- **🏠 [Homepage](https://gethomepage.dev/)** - Unified dashboard with service integrations and widgets ([GitHub](https://github.com/gethomepage/homepage))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/n8n.svg" width="16" height="16"> [n8n](https://n8n.io/)** - Workflow automation platform with 400+ integrations ([Installation Guide](https://docs.n8n.io/hosting/) | [GitHub](https://github.com/n8n-io/n8n))

**AI & Compute:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/ollama.svg" width="16" height="16"> [Ollama + Open WebUI](https://openwebui.com/)** - Self-hosted LLM inference and chat UI on RTX 5080 GPU ([GitHub](https://github.com/open-webui/open-webui))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [Idea Generator](https://github.com/wielandtech-labs/idea-generator)** - Business idea vetting, market research, and loan proposal generation (Claude + local Ollama backend)

**Security:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/vaultwarden.svg" width="16" height="16"> [Vaultwarden](https://github.com/dani-garcia/vaultwarden)** - Self-hosted Bitwarden-compatible password manager

**Gaming:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/minecraft.svg" width="16" height="16"> [Minecraft Server](https://papermc.io/)** - Paper Java server ([GitHub](https://github.com/itzg/docker-minecraft-server))

**Infrastructure & DevOps:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/postgresql.svg" width="16" height="16"> [PostgreSQL](https://www.postgresql.org/)** - Production-ready database clusters with CloudNativePG ([Installation Guide](https://www.postgresql.org/docs/current/installation.html) | [GitHub](https://github.com/postgres/postgres))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/redis.svg" width="16" height="16"> [Redis](https://redis.io/)** - In-memory data store for caching and sessions ([Installation Guide](https://redis.io/docs/getting-started/installation/) | [GitHub](https://github.com/redis/redis))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/renovate.svg" width="16" height="16"> [Renovate](https://renovatebot.com/)** - Automated dependency updates and security patches ([Documentation](https://docs.renovatebot.com/) | [GitHub](https://github.com/renovatebot/renovate))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/ntfy.svg" width="16" height="16"> [ntfy](https://ntfy.sh/)** - Self-hosted push notifications; delivery channel for all cluster alerts ([GitHub](https://github.com/binwiederhier/ntfy))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/temporal.svg" width="16" height="16"> [Temporal](https://temporal.io/)** - Workflow orchestration engine ([GitHub](https://github.com/temporalio/temporal))

**Web Services & Projects:**
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [raphaelwieland.com](https://raphaelwieland.com/)** - Personal site: resume, blog, photos, and accounts ([GitHub](https://github.com/wielandtech-labs/w_tech))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [wielandtech.com](https://wielandtech.com/)** - Static Astro portfolio for VLAN Technology Services ([GitHub](https://github.com/wielandtech-labs/w_portfolio))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [WARN Notice Index](https://warnindex.com/)** - Public WARN Act layoff filings aggregator, search index, PDF archive, and ML enrichment pipeline ([GitHub](https://github.com/wielandtech-labs/warn_scraper_v2))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/spotify.svg" width="16" height="16"> [Democra-Party](https://party.wielandtech.com/)** - Spotify-backed music room with democratic queue voting ([GitHub](https://github.com/wielandtech-labs/democra-party))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [r12d](https://r12d.dev/)** - Privacy-first short links, QR stickers, photo inventory, and light CRM ([GitHub](https://github.com/wielandtech-labs/r12d))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [Reseller](https://github.com/wielandtech-labs/w_reseller)** - White-label CRM for used-goods resellers with comp-backed quotes and deal pipeline
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [Pisspoor](https://pisspooridea.lol/)** - Restroom QR ad network: per-venue QR short links, ratings, and revenue split ([GitHub](https://github.com/wielandtech-labs/pisspoor))
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [Fluck](https://fluck.r12d.ch/)** - Public ALPR / Flock camera map and dataset viewer
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [Shop](https://github.com/wielandtech-labs/w_shop)** - Ecommerce store for shirts, stickers, and 3D prints (Medusa v2 + Next.js)
- **<img src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/svg/github.svg" width="16" height="16"> [github-readme-stats](https://stats.wielandtech.com/)** - Self-hosted GitHub README stats and top-languages SVG card generator

## 🤖 Agent Development Platform
Since mid-2026 the cluster doubles as a deployment target for AI coding agents (Claude Code) building full applications end to end. The private GitOps repo provides the rails:

- **One-command app scaffolding** — manifests, Flux image automation, secrets, DNS conventions
- **Per-PR review apps** — every pull request gets its own namespace, ephemeral database, TLS certificate, and `pr-N.review.*` URL, torn down on close; locked behind default-deny NetworkPolicies; conflict-free kustomization with automatic stale-PR regeneration
- **dev → QA → prod promotion** — image-tag-based, gated by CI and post-merge deploy verification on in-cluster runners
- **Read-only agent access** — agents debug deployments through a Tailscale API-server proxy and a custom read-only MCP server, both bound to a least-privilege ClusterRole (no secrets, no writes; all changes go through pull requests)
- **Local LLM inference** — Ollama on RTX 5080 GPU for cost-effective local model calls alongside Claude API

## 🔗 Repository Links
- **📸 Documentation & Hardware**: [w_lab](https://github.com/wielandtech-labs/w_lab) (this repo) - Public documentation, hardware list, and photos
- **🔧 Infrastructure Code**: Private GitOps repository with the Kubernetes manifests and Flux configuration (Flux v2, CloudNativePG, Prometheus, sealed secrets)
- **🌐 Personal Website**: [raphaelwieland.com](https://raphaelwieland.com/) - Personal site, resume, blog, and photos ([Source Code](https://github.com/wielandtech-labs/w_tech))
- **🏢 Portfolio**: [wielandtech.com](https://wielandtech.com/) - VLAN Technology Services portfolio ([Source Code](https://github.com/wielandtech-labs/w_portfolio))

## 🙏 Special Thanks
* [**Jeff Geerling**](https://github.com/geerlingguy) — ["Project Mini Rack"](https://github.com/geerlingguy/mini-rack) for inspiring my shopping list.
* [**Mischa van den Burg**](https://github.com/mischavandenburg) — ["K8S Homelab"](https://github.com/mischavandenburg/homelab) for inspiring my stack.

### 🖨️ 3D Print Files
* [**Mauker**](https://www.printables.com/@Mauker_) — [Unifi USW-Ultra 10-inch Rack Mount](https://www.printables.com/model/963100-unifi-usw-ultra-10-inch-rack-mount), [Unifi UCG-Fiber 10-inch Rack Mount](https://www.printables.com/model/1235928-unifi-ucg-fiber-10-inch-rack-mount), [10-inch 1U Keystone Patchpanel x8 ports](https://www.printables.com/model/1180979-10-inch-12u-keystone-patchpanel-x8-ports)
* [**TimPrints**](https://www.printables.com/@TimPrints_686384) — [Dell OptiPlex 7060 MicroPC 10 Inch Rack Mount](https://www.printables.com/model/980541-dell-optiplex-7060-micropc-10-inch-rack-mount)

## 📸 Gallery
<table>
  <tr>
    <td align="center" width="50%">
      <a href="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_front.jpg" target="_blank">
        <img src="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_front.jpg" alt="Homelab Front View" width="400" style="max-width: 100%; height: auto;">
      </a>
      <br><em>Front View</em>
    </td>
    <td align="center" width="50%">
      <a href="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_back.jpg" target="_blank">
        <img src="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_back.jpg" alt="Homelab Back View" width="400" style="max-width: 100%; height: auto;">
      </a>
      <br><em>Back View</em>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <a href="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_left.jpg" target="_blank">
        <img src="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_left.jpg" alt="Homelab Left Side View" width="400" style="max-width: 100%; height: auto;">
      </a>
      <br><em>Left Side View</em>
    </td>
    <td align="center" width="50%">
      <a href="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_right.jpg" target="_blank">
        <img src="https://raw.githubusercontent.com/wielandtech-labs/w_lab/main/w_homelab_right.jpg" alt="Homelab Right Side View" width="400" style="max-width: 100%; height: auto;">
      </a>
      <br><em>Right Side View</em>
    </td>
  </tr>
</table>
