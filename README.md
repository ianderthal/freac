## Optical drive device path

The USB optical drive's `/dev/srX` path can differ per host (e.g. `/dev/sr1`
on the Proxmox VM, `/dev/sr0` on the mini PC), so it's set via an env var
instead of hardcoded:

```yaml
devices:
  - ${OPTICAL_DRIVE_PATH:-/dev/sr0}:/dev/sr0
```

Set `OPTICAL_DRIVE_PATH` in each Portainer stack's **Environment variables**:

| Host | `OPTICAL_DRIVE_PATH` |
|---|---|
| Homelab | `/dev/sr1` |
| Production | `/dev/sr0` (or omit) |

Verify on the host after deploying:
```bash
docker inspect freac | grep -A 5 Devices
```