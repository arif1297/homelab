## Overview

After rebooting my server, I was unable to ssh from my laptop. The issue was caused by the server’s networking configuration, which prevented the server from properly connecting to the network.

This troubleshooting process involved diagnosing the networking issue locally on the server, correcting the network configuration, and restoring remote ssh access.

---

# The Problem

When attempting to ssh into the server, the connection failed.

Initial tests showed: host unreachable

This indicated that the laptop could not communicate with the server over the network.

Since ssh relies on the server being connected to the network correctly, remote access was unavailable until the networking issue was resolved.

---

# Diagnosing the Issue

To investigate further, I accessed the server locally using a monitor and keyboard.

The server’s ip configuration was checked using:

```bash
ip a
```

This helped confirm whether the server had received a valid ip address from the router.

The output showed that the primary network interface:

```bash
enp1s0
```

was in a `DOWN` state and had not been assigned a valid LAN ip address.

Only localhost and Docker bridge addresses were present, meaning the server had not successfully connected to the local network.

---

# Temporary Network Fix

To restore connectivity temporarily, the network interface was manually brought online using:

```bash
sudo ip link set enp1s0 up
```

A DHCP lease was then requested from the router:

```bash
sudo dhclient enp1s0
```

After running these commands:
- server received a valid local ip address
- ping requests succeeded
- ssh access was restored successfully

This confirmed that the issue was related to the server’s network configuration during boot rather than a hardware fault.

---

# Investigating Netplan Configuration

Ubuntu Server uses Netplan to manage networking.

The existing configuration file was checked and contained:

```yaml
network:
  ethernets: {}
  version: 2
```

This meant:
- No ethernet interfaces were configured
- Ubuntu was not automatically bringing `enp1s0` online during boot

The configuration was also being controlled by `cloud-init`, meaning changes would not persist after rebooting.

This explained why the networking issue returned every time the server restarted.

---

# Permanent Resolution

## Disabling Cloud-Init Network Overrides

To stop cloud-init from overwriting the network configuration, the following file was created:

```bash
sudo nano /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```

The following configuration was added:

```yaml
network: {config: disabled}
```

---

## Creating a Persistent Netplan Configuration

A new Netplan configuration file was then created:

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Configuration used:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: true
```

---

## Applying the Configuration

The configuration changes were applied using:

```bash
sudo netplan generate
sudo netplan apply
```

The file permissions were also corrected:

```bash
sudo chmod 600 /etc/netplan/01-netcfg.yaml
```

---

# SSH Configuration

To ensure remote access remained available after rebooting, the ssh service was configured to automatically start during boot:

```bash
sudo systemctl enable ssh
```

---

# Final Outcome

After reboot testing:
- The network interface automatically came online
- The server successfully received an ip address from the router
- ssh became accessible immediately after boot
- The server could now be managed remotely without requiring a monitor or keyboard plugged in

The server is now configured for reliable headless management using ssh from my laptop

---
