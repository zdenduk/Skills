---
name: debug-ts6-server
description: Debug a self-hosted TeamSpeak 6 server over SSH using a local, private server profile. Use when Codex is asked to diagnose TS6 availability, ports, Docker or Compose state, logs, licensing, persistence, network/firewall exposure, or startup failures.
---

# Debug TS6 Server

## Privacy Model

This public skill intentionally avoids real hostnames, IP addresses, usernames, private key paths, key filenames, and provider-specific account details.

Keep private connection details in `references/server-profile.local.md`, copied from `references/server-profile.example.md`. Do not commit the local profile or any private key material.

## Workflow

1. Read `references/server-profile.local.md` before connecting or suggesting host-specific fixes. If it is missing, read `references/server-profile.example.md` and ask the user for the missing private connection details.
2. Start with read-only diagnostics: SSH reachability, host health, container or service state, logs, ports, disk, memory, and firewall exposure.
3. Treat the remote host as live infrastructure. Do not restart services, change firewall rules, delete containers, remove volumes, prune Docker resources, modify databases, or upgrade packages without explicit user approval.
4. Capture current state before any requested change: relevant logs, `docker ps`, port listeners, disk space, and the exact command you plan to run.
5. Prefer evidence over guesses. If the issue may be outside the host, say so clearly, especially for cloud ingress/security-list, DNS, routing, or client-side connectivity problems.

## Connection

Use PowerShell-compatible SSH commands from the local machine:

```powershell
ssh -i "<path-to-private-key>" <ssh-user>@<server-host>
```

For one-off read-only probes:

```powershell
ssh -i "<path-to-private-key>" <ssh-user>@<server-host> 'hostname; uptime; df -h; free -h'
```

If SSH fails, check the local key path first, then network reachability, then whether the SSH user or host changed.

## Diagnostic Order

1. Confirm host basics: `hostname`, `uptime`, `df -h`, `free -h`, `uname -a`.
2. Detect how TS6 runs: Docker container, Docker Compose project, or direct binary/systemd service.
3. Inspect process and port state. TS6 commonly uses UDP `9987` for voice, TCP `30033` for file transfer, and optional TCP `10080` for Web Query.
4. Inspect logs for startup errors, license acceptance, database errors, bind failures, and privilege key messages.
5. Verify persistence. Docker examples commonly use `/var/tsserver` in the container and a named volume for server data.
6. Check host firewall rules and cloud ingress exposure when local listeners are correct but clients cannot connect.
7. Use README-derived beta constraints before promising unsupported fixes: TS6 self-hosted server files are beta, TS3 licenses are not compatible, and there is no TS3-to-TS6 migration path.

## Reporting

Report findings with:

- Current status: reachable or not, container/service state, listening ports, and latest relevant log lines.
- Most likely cause, tied to command output.
- Next action, separating safe read-only checks from actions that need approval.
- Exact remote command for any proposed state-changing fix.
