# Issue: Nextcloud Not Accessible Due to Port Configuration

## Observed Behaviour
- Docker containers started successfully
- Nextcloud was not accessible via web browser
- Browser connection attempts to the service failed

## Investigation
Checked the status of running containers:

```bash
docker ps
```
Checked service status using Docker Compose:
```bash
docker compose ps
```
Checked which ports were currently in use on server:
```bash
ss -tulpn
```
Tested access locally from the server:
```bash
curl http://localhost:<port>
```
## Resolution
Updated the port mapping in the docker-compose.yml file to use an available port.
Restarted the containers to apply the change:
```bash
docker compose down
docker compose up -d
```
Verified containers were running after restart:
```bash
docker ps
```
## Outcome
Nextcloud became accessible via the correct server IP address and port
The web interface loaded successfully and functioned as expected
