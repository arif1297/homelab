# Issue: Unable to SSH into server after system updates

## Issues Observed
- SSH access failed after rebooting the server following system updates
- Error returned: `No route to host`
- Server was powered on but unreachable over the network
- No internet access on the server
- Ethernet LEDs only active during reboot, then turned off

---

## Investigation

Attempted SSH connection from another device:
```bash
ssh user@<server-ip>
```

Checked whether the network interface was detected and active:
```bash
ip link
ip addr
```

Confirmed the Ethernet interface existed but was down:
```bash
enp1s0 state DOWN
```

Checked routing table:
```bash
ip route
```

Tested internet connection:
```bash
ping 8.8.8.8
```

---

## Resolution

Manually brought the Ethernet interface up:
```bash
sudo ip link set enp1s0 up
sudo dhclient enp1s0
```

This restored internet connection temporarily.

To ensure the interface is automatically enabled on boot, the network configuration was updated using netplan:
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Applied the following configuration:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: true
      optional: false
```

Applied the configuration:
```bash
sudo netplan generate
sudo netplan apply
```

---

## Outcome
- Ethernet interface now comes up automatically on boot
- IP address is assigned via DHCP
- Internet connection restored
- SSH access works reliably again

---
