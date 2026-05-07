# Issue: No Internet Connection After Ubuntu Installation

## Issues Observed
- Ethernet cable was connected to the server
- No internet access after installing Ubuntu Server
- Commands requiring internet access failed

## Investigation
Checked whether the network interface was detected and had an IP address:

```bash
ip link
ip addr
```
Confirmed the default route was missing
```bash
ip route
```
Pinged Google to confirm there was no internet connection:
```bash
ping 8.8.8.8
```
## Resolution
Manually enabled the Ethernet interface:
```bash
sudo ip link set eth0 up
```
Tested internet connection again:
```bash
ping 8.8.8.8
```
## Outcome
The Ethernet interface became active
Internet connection was restored
The server was able to download updates and packages successfully
Was able to continue configuring the server
