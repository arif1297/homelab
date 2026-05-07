# CasaOS setup

CasaOS installed and configured alongside an existing Docker-based Nextcloud environment running on Ubuntu Server.

---

# Project status

- CasaOS successfully installed
- Existing Nextcloud Docker containers preserved
- Docker integration confirmed working
- Multiple services running simultaneously without port conflicts
- Legacy Docker containers detected inside CasaOS

---

# Overview

The goal of this setup was to introduce CasaOS into an already functioning Ubuntu Server environment without disrupting the existing Nextcloud deployment.

The server already contained:
- Docker
- Nextcloud
- MariaDB

CasaOS was installed as a management layer on top of the existing environment.

---

# Existing environment

The following command was used to inspect existing Docker containers:

```bash
docker ps
```

This confirmed:
- `nextcloud_app_1`
- `nextcloud_db_1`

were already running prior to installing CasaOS.

---

# CasaOS installation

CasaOS was installed using the official installation script:

```bash
curl -fsSL https://get.casaos.io | sudo bash
```

The installation:
- detected the existing Docker installation,
- installed CasaOS services,
- and created a web-based dashboard.

---

# Accessing CasaOS

CasaOS was accessed through:

```text
http://<server-ip>
```

The initial setup included:
- creating login credentials,
- configuring the dashboard,
- and confirming Docker integration.

---

# Running CasaOS alongside Nextcloud

The existing Nextcloud deployment was intentionally left unchanged.

Instead of reinstalling Nextcloud through CasaOS:
- the original Docker containers were preserved,
- and CasaOS was used only for managing new applications.

This avoided:
- data loss risks,
- duplicate services,
- and unnecessary migration complexity.

---

# Legacy applications

After installation, CasaOS detected the existing Docker containers as:

> "Legacy Apps (To be rebuilt)"

This indicated:
- the containers already existed,
- but were not originally created by CasaOS.

The services continued functioning normally while remaining externally managed.

---

# Port management

Careful consideration was required to avoid port conflicts between services.

Current layout:

| Service | Port |
|---|---|
| CasaOS Dashboard | 80 |
| Nextcloud | 8080 |

This allowed both services to run simultaneously without interference.

---

# Understanding CasaOS

CasaOS does not replace Ubuntu Server or Docker.

Instead, CasaOS acts as:
- a dashboard,
- a Docker management interface,
- and a simplified application deployment platform.

Architecture overview:

```text
Ubuntu Server
│
├── Docker
│   ├── Nextcloud
│   ├── MariaDB
│   └── CasaOS Applications
│
└── CasaOS Dashboard
```

---

# Key learning areas

- Docker container management
- Docker networking
- Port mapping and conflict avoidance
- Multi-service server management
- Understanding how CasaOS integrates with Docker
- Managing self-hosted applications

---

# Result

Successfully achieved:
- a functioning CasaOS deployment,
- preservation of the existing Nextcloud environment,
- and a scalable platform for future homelab services.
