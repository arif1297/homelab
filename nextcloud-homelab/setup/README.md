# Nextcloud Deployment Setup

## Overview
This homelab project documents the deployment of a self-hosted Nextcloud instance running on Ubuntu Server.
The server was configured for remote management using SSH, with Nextcloud and its supporting services deployed using Docker containers.

The aim of this setup was to gain hands-on experience with Linux administration, containerisation, and self-hosted services in a homelab environment.

---

## Operating System: Ubuntu Server
Ubuntu Server was installed as the base operating system due to its stability and long-term support.
A minimal installation was used to reduce unnecessary services and keep the system lightweight.

---

## Remote Management via SSH
SSH was configured on the server to allow secure remote access from my laptop.
This enabled all tasks to be carried out remotely without requiring physical access to the server.

---

## Containerisation with Docker
Docker was installed to run Nextcloud and its dependencies in isolated containers.
This approach keeps application services separate from os and makes recovery easier if something goes wrong.

A docker-compose configuration was used to define and manage Nextcloud and its required services.

---

## Nextcloud Services
The deployment consists of:
- A Nextcloud application container
- A database container used by Nextcloud
- Docker volumes to persist application and user data

Using volumes ensures that data is retained even if containers are restarted or recreated.

---

## Access and Validation
The Nextcloud service is accessed through a web browser using the server’s IP address and configured port.

Deployment was validated by:
- Confirming containers were running
- Accessing the Nextcloud web interface
- Logging in and uploading test files

---

## Current Limitations
This setup focuses on core functionality and learning outcomes.

The following areas are planned for future improvement:
- Automated backups
- VPN-based remote access
- Additional security hardening
- User administration and onboarding

