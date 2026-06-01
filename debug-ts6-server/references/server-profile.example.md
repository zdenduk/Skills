# TS6 Server Profile Example

Copy this file to `server-profile.local.md` and replace the placeholders with private values. Keep the local profile uncommitted.

## Fixed Access

Use this SSH command from PowerShell:

```powershell
ssh -i "<path-to-private-key>" <ssh-user>@<server-host>
```

Use this pattern for one-off remote probes:

```powershell
ssh -i "<path-to-private-key>" <ssh-user>@<server-host> '<remote command>'
```

## TeamSpeak 6 Facts

- TS6 self-hosted server is beta software; unstable or changing features are expected.
- TeamSpeak 3 server licenses are not compatible with TeamSpeak 6.
- There is no TS3-to-TS6 migration path noted in the public TS6 server materials.
- The beta server includes a 32-slot beta license renewed during the beta/evaluation period.
- Configuration can come from command-line arguments, environment variables, or `tsserver.yaml`.
- A default config can be generated with `--write-config-file`.
- Complete option discovery is available with `--help` or local `CONFIG.md`.

## Docker Defaults

Common image:

```text
teamspeaksystems/teamspeak6-server:latest
```

Common container name:

```text
teamspeak-server
```

Common ports:

```text
9987/udp    voice
30033/tcp   file transfer
10080/tcp   optional Web Query
```

Common data path and volume:

```text
/var/tsserver
teamspeak-data
```

Required license acceptance environment variable:

```text
TSSERVER_LICENSE_ACCEPTED=accept
```

## Safe Read-Only Remote Checks

Host health:

```sh
hostname
uptime
df -h
free -h
uname -a
```

Docker inventory:

```sh
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}'
docker ps -a --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}'
docker images | grep -i teamspeak || true
docker volume ls | grep -i teamspeak || true
```

Logs, when the container name matches the default:

```sh
docker logs --tail 200 teamspeak-server
```

Compose discovery:

```sh
find ~ /opt /srv -maxdepth 4 \( -name 'compose.yaml' -o -name 'docker-compose.yaml' -o -name 'docker-compose.yml' \) 2>/dev/null
```

Port listeners:

```sh
sudo ss -lunpt | grep -E '(:9987|:30033|:10080)' || true
```

Firewall snapshots:

```sh
sudo firewall-cmd --list-all 2>/dev/null || true
sudo iptables -S 2>/dev/null | head -200
sudo nft list ruleset 2>/dev/null | head -200
```

Systemd discovery if not Docker:

```sh
systemctl list-units --type=service --all | grep -Ei 'teamspeak|ts6|tsserver' || true
ps aux | grep -Ei '[t]eamspeak|[t]sserver'
```

## State-Changing Actions Requiring Explicit Approval

Ask before running any of these or equivalents:

- `docker restart`, `docker stop`, `docker start`, `docker compose restart`, `systemctl restart`
- `docker rm`, `docker compose down`, `docker volume rm`, `docker system prune`
- firewall changes, package installs/upgrades, file edits under server data paths
- database migrations, database deletion, or manual edits to persistent TS6 state

Before an approved restart or config change, capture:

```sh
docker ps -a
docker logs --tail 200 teamspeak-server
sudo ss -lunpt | grep -E '(:9987|:30033|:10080)' || true
df -h
```
