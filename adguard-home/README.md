# AdGuard Home configuration

AdGuard Home configured on Ubuntu Server through CasaOS to provide DNS-based ad blocking and domain filtering across the local network.

---

# Project status

- AdGuard Home successfully deployed through CasaOS
- DNS filtering functional
- Port 53 conflict resolved
- iPhone manually configured to use custom DNS
- Real-time DNS query monitoring confirmed working

---

# Overview

The goal of this project was to understand how DNS filtering works and configure a self-hosted DNS filtering solution using AdGuard Home.

The setup was deployed alongside:
- Ubuntu Server
- Docker
- CasaOS
- Nextcloud

---

# Installing AdGuard Home

AdGuard Home was installed through the CasaOS App Store.

The application runs inside Docker and provides:
- DNS filtering,
- query monitoring,
- ad blocking,
- and custom domain filtering.

---

# Understanding DNS

DNS (Domain Name System) acts as the "phonebook of the internet".

When a device accesses a website such as:

```text
google.com
```

the device must first request the IP address associated with that domain.

Normally this request goes to:
- the router,
- ISP DNS,
- or public DNS providers.

With AdGuard Home configured, DNS traffic instead flows through the local server first.

---

# DNS traffic flow

Before configuration:

```text
Phone -> Router DNS -> Internet
```

After configuration:

```text
Phone -> AdGuard Home -> Internet
```

This allows AdGuard Home to:
- inspect DNS requests,
- block specific domains,
- and filter ads before websites are accessed.

---

# Initial Problem: Port 53 Conflict

During installation, AdGuard Home could not properly function because port 53 was already in use.

Port 53 is required for DNS traffic.

---

# Diagnosing the Issue

The following command was used to identify services using port 53:

```bash
sudo ss -tulnp | grep :53
```

Output showed:

```text
127.0.0.53:53
systemd-resolved
```

This confirmed Ubuntu's built-in DNS resolver (`systemd-resolved`) was occupying the required port.

---

# Resolving the Port Conflict

The built-in resolver service was disabled:

```bash
sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved
```

---

# Restoring DNS Resolution

After disabling `systemd-resolved`, manual DNS configuration was required.

Existing resolver configuration was removed:

```bash
sudo rm /etc/resolv.conf
```

A new resolver configuration was then created:

```bash
sudo nano /etc/resolv.conf
```

The following DNS server was added:

```text
nameserver 1.1.1.1
```

This temporarily restored DNS resolution through Cloudflare DNS.

---

# Docker Port Mapping Issue

Initially, AdGuard Home was incorrectly mapped to:

```text
531 -> 53
```

This prevented devices from communicating with the DNS service because standard DNS traffic uses port 53.

---

# Correct Port Mapping

The configuration was updated to:

```text
53 -> 53 (TCP)
53 -> 53 (UDP)
```

This allowed AdGuard Home to correctly receive DNS traffic from client devices.

---

# Verifying DNS Service

The following command confirmed AdGuard Home was successfully listening on port 53:

```bash
sudo ss -tulnp | grep :53
```

Expected output:

```text
0.0.0.0:53
```

---

# Client Device Configuration

To test functionality, an iPhone was manually configured to use the server as its DNS provider.

## iPhone DNS setup

Settings → WiFi → Configure DNS → Manual

The server IP address was added as the DNS server.

This redirected DNS traffic through AdGuard Home instead of the router or ISP DNS.

---

# Understanding Why Manual DNS Was Required

By default, devices use the router's DNS server automatically.

This means AdGuard Home would never receive DNS traffic unless devices were explicitly configured to use it.

Changing the DNS settings instructed the device to send DNS requests directly to the local server.

---

# Understanding How AdGuard Home Works

AdGuard Home acts as a DNS filter.

When a domain is requested:
- allowed domains are resolved normally,
- blocked domains are denied.

Allowed example:

```text
Device -> "Where is google.com?"
AdGuard -> returns IP address
```

Blocked example:

```text
Device -> "Where is ads.example.com?"
AdGuard -> denies request
```

The website cannot load because the client device never receives the destination IP address.

---

# Query Monitoring

The AdGuard Home dashboard provides:
- real-time DNS query monitoring,
- blocked request statistics,
- connected client visibility,
- and filtering logs.

This was used to verify:
- client devices were using the DNS server,
- filtering was functioning correctly,
- and DNS requests were successfully processed.

---

# Key learning areas

- DNS fundamentals
- Linux service management
- Docker networking and port mapping
- Port conflict troubleshooting
- DNS filtering concepts
- Network-level ad blocking
- Client DNS configuration
- Real-time DNS monitoring

---

# Result

Successfully achieved:
- functioning DNS filtering,
- real-time DNS monitoring,
- and a working self-hosted AdGuard Home deployment running alongside existing Docker services.
